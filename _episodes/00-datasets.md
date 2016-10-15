---
title: "datasets for the xarray tutorial"
teaching: 0
exercises: 0
questions:
- "What sample datasets will we use in this tutorial?"
objectives:
- "Learn about how we acquired climate reanalysis datasets"
keypoints:
- "refer to this page for access to the tutorial data"
---

### Reanalysis datasets:

We will be exploring the xarray architecture using some sample climate data from the European Centre for Medium-Range Weather Forecasts ([ECMWF](http://www.ecmwf.int/)). We will use their ERA-Intrim climate reanalysis project. You can download the data in netcdf format [here](http://apps.ecmwf.int/datasets/data/interim-full-daily/levtype=sfc/). As is the case for many climate products, the process involves downloading large netcdf files to a local machine.

(note: many of our examples follow from and expand on xarray developer Stephan Hoyer's [blog post](https://www.continuum.io/content/xray-dask-out-core-labeled-arrays-python)).

If you visit the ECMWF page you will find that you can download a large number of different climate fields. Here we have prepared tutorial examples around 4 variables. Note that we provide a full resolution (global) version as well as a subsetted version (Alaska). Choose the Alaska data if you are limited for space or processing power on your local computer. The tutorials exercises will work for either set of data.


| Dataset | Description | Size (MB)
|:-------------:|:-------------:|:------------:|
| | | |
| airtemp_global.nc | 2 meter air temperature | 3.0 |
| uwind_global.nc | wind blowing to the east | 3.0 |
| vwind_golbal.nc | wind blowing to the north | 3.0 |
| SST_global.nc | sea surface temperature | 3.0 |
| | | |
| airtemp_AK.nc | 2 meter air temperature | 0.1 |
| uwind_AK.nc | wind blowing to the east | 0.1 |
| vwind_AK.nc | wind blowing to the north | 0.1 |
| SST_AK.nc | sea surface temperature | 0.1 |