1. install conda, in MAC/Linux/Windows -> download sh and execute: https://www.anaconda.com/products/individual (command line installers)

	Conda commands:
	- To view packages installed in conda: `conda list`
	- To view environments in conda: `conda env list`
	- To create a new environment by name: `conda create -n <name>`.  Example: `conda create -n insitu`
	- To activate the env: `conda activate insitu`
	- To deactivate the env: `conda deactivate`
	- To search for packages: `conda search <package>`. Example: `conda search netcdf`
	- To export environment: `conda env export -n test-env -f test-env-spec.yml`
	- To import environment: `conda env create -n test-env-replica -f=test-env-spec.yml`
	- To delete environment: `conda env remove -n test-env`

2. We need to install these python (3+) libraries:
   jupyter, xarray, netcdf, pandas, urllib, json, requests, bs4, ipywidgets, collections, uuid, re, dask


3. To copy the files into the folder locally:
   scp -r <local_folder> vyathira@oiip:/data/home/vyathira


To run jupyter notebook on oiip server:
ssh vyathira@137.78.248.130

Run this ssh forwarding command on local machine (so that jupyter notebook run in oiip server can be accessed locally):
ssh -L 8889:localhost:8889 oiip

In oiip server, run the following:
conda activate insitu-env
jupyter notebook

Copy the URL that contains token (for example: localhost:8889./token=....) and run it in local browser to access the notebook.

