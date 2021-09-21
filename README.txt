Introduction:

This is a tool to extract SPURS data from PODAAC api (https://podaac.jpl.nasa.gov/api/cmr/). The SPRUS data which is in netcdf format is loaded as python data frame and then exported to a CSV file.

Using this tool we can select one or more datasets. From the selected datasets we can then select one or more files with coordinates and variables that we are interested in. We can also choose to export the number of rows for our desired selection of datasets/files/coordinates/variables.

(Note: if we select too many files or if the file size is too large then it may be difficult to load into the memory and the jupyter notebook would timeout/crash.)

Data:

There are ~28 datasets. Each dataset has multiple files. The file sizes vary from few KB to ~100TB. Each netcdf file has some coordinates (coords) and some variables (vars). We have finalized the list of coords and vars that are going to be exported (see preprocess/final_header.txt). The common coords that we are interested in are time, latitude, longitude, depth.

There are few different names for the variables.
variable_name (or var or symbol): for example, u, v, lat, lon, relative_humidity_at_10m etc
netcdf_standard_name (or standard_name): for example, east component of wind, north component of wind, latitude, longitude, relative humidity etc
variable_standard_name_CF (remapped based on CF or internal standard): eastward_wind, northward_wind, latitude, longitude, relative_humidity etc

Depth Assignment:

Some variables represent measurement at a specific depth for example relative_humidity_at_10m. A file might have few such variables, it is possible to have relative_humidity_at_10m, relative_humidity_at_25m in same file, so we try to capture the depth assignment values in separate columns depth_10, depth_25 etc and are considered as coordinates.

The exported CSV will have coordinates and the remapped variable standard names. The coordinates are prefixed with 'coordinate_'. So we have coordinates like coordinate_time, coordinate_latitude, coordinate_longitude, coordinate_depth, coordinate_depth_-15, coordinate_depth_-5, coordinate_depth_0.0, coordinate_depth_10, coordinate_depth_25 , etc

Preprocess:

The following information is preprocessed before clients try to access the main notebook.ipynb:

- For each dataset a UUID is generated and stored in preprocess/datasetUUIDs.txt
- For each file a UUID is generated and stored in preprocess/fileUUIDs.txt
- The column names (or header) for CSV is generated and stored in preprocess/final_header.txt (Note that there is some manual process to select/order the columns)
- variable_name * standard_name : remapped_name mapping is populated and stored in proprocess/remapped_variables.txt
- variable_name * standard_name : depth_assignment mapping is populated and stored in preprocess/varz_to_depth_assignment.txt

Notebook Code (notebook.ipynb):

Step 1: 
- Load all the preprocess data as python dictionaries.
- Query PODAAC api and populate URL -> dataset name map.

Step 2:
- Display dataset names in ipython widget, so that users can select one or more datasets.
- (Note: We would suggest to choose only one dataset at a time as we more data might crash jupyter notebook)

Step 3:
- Loads selected dataset(s) into memory
- For each file in the selected dataset, reads netcdf data and converts to python dataframe
- Stores coords/vars for each file in python dictionary
- Stores dataframe for each file in python dictionary

Step 4:
- Display file names (along with coordinates and variables) in ipython widget, so that users can select one or more files (and corresponding coords/variables)

Step 5:
- If only variables are selected in a file, we make sure to select all coordinates for those file
- If we choose to select only the variables, then set should_include_all_coords to False.

Step 6 (optional, for debugging purpose):
- prints selected coords, selected variables in the selected files.
- prints all coords, all variables in the selected files.

Step 7:
- Creates output/spurs_data.csv
- Writes header columns (includes coordinates and remapped variables) in the output CSV file.

Step 8:
- Writes data to the output/spurs_data.csv file.
- Use max_records_per_file to select the number of records for the data to be exported for each file. For example if max_records_per_file is set to 3, only 3 records are exported for each file.
- If max_records_per_file is set to -1, then entire data is exported.

Note: each time we run the jupyter notebook the output file is overwritten, so make sure to rename/copy the old data.