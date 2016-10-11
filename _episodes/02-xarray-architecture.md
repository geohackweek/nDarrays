---
title: "xarray architecture"
teaching: 20
exercises: 10
questions:
- "What functionality does the xarray library offer?"
- "What are the benefits and limitations of this library?"
- "What is the fundamental architecture of xarray data objects?"
objectives:
- "learning the xarray data model"
- "selection and subsetting of array datasets using labeled indexing"
keypoints:
- "xarray is build on the NetCDF data model"
- "xarray has two main data structures: DataArray and Dataset"
- "DataArrays store the multi-dimensional arrays"
- "Datasets are the multi-dimensional equivalent of a Pandas dataframe"
---

### What is xarray?

* originally developed by employees (Stephan Hoyer, Alex Kleeman and Eugene Brevdo) at [The Climate Corporation](https://climate.com/)
* xaray extends some of the core functionality of Pandas:
    * operations over _named_ dimensions
    * selection by label instead of integer location
    * powerful groupby functionality
    * database-like joins

### When to use xarray:

* if your data are multidimensional (e.g. climate data: x, y, z, time)
* if your data are structured on a regular grid
* if you can represent your data in NetCDF format

### Basic xarray data structures:
* NetCDF forms the basis of the xarray data structure
* two main data structures are the _DataArray_ and the _Dataset_

#### DataArray
* the DataArray is xarray's implementation of a labeled, multi-dimensional array
* the DataArray has these key properties:
  * _data_: N-dimensional array (NumPy or dask) holding the array's values,
  * _ dims_: dimension names for each axis,
  * _coords_: dictionary-like container of arrays that label each point, and
  * _attrs_: ordered dictionary holding metadata

<br><br><br>
<img src="http://xray.readthedocs.org/en/stable/_images/dataset-diagram.png" width = "800" border = "10">
<br><br><br>
* dimensions (x, y, time); variables (temp, precip); coords (lat, long); attributes


### Dataset
* xarray's multi-dimensional equivalent of a Pandas DataFrame
* dict-like container of DataArray objects with aligned dimensions
* Datasets have these key properties:
  * _ dims_: dictionary mapping from dimension names to the fixed length of each dimension,
  * _data_vars_: dict-like container of DataArrays corresponding to data variables,
  * _coords_: dictionary-like container of DataArrays intended to label points used in data_vars
  * _attrs_: ordered dictionary holding metadata

### sample dataset

Let's explore the xarray architecture using some sample climate data from the European Centre for Medium-Range Weather Forecasts ([ECMWF](http://www.ecmwf.int/)). We will use their ERA-Intrim climate reanalysis project. You can download the data in netcdf format [here](http://apps.ecmwf.int/datasets/data/interim-full-daily/levtype=sfc/). As is the case for many climate products, the process involves downloading large netcdf files to a local machine.

Note: this example follows and expands from xarray developer Stephan Hoyer's [blog post](https://www.continuum.io/content/xray-dask-out-core-labeled-arrays-python).

### begin by importing the xarray library

{% highlight python %}
import xarray
{% endhighlight %}

### Open the dataset

First we [open](http://xarray.pydata.org/en/stable/generated/xarray.open_dataset.html) the data and load it into a *Dataset*. (Note: the choice of engine depends on the format of the netcdf file).

~~~
ds = xarray.open_dataset('<rootDir>/ecmwf_airtemp.nc', engine = 'scipy')
~~~
{: .python}

You'll notice this seemed to go very fast. That is because this step does not actually ask Python to read the data into memory. Rather, Python is just scanning the contents of the file. This is called _lazy evaluation_. 

## Dataset Properties

Next we will ask xarray to display some of the [parameters](http://xarray.pydata.org/en/stable/generated/xarray.Dataset.html) of the Dataset. To do this simply return the contents of the Dataset variable name:

~~~
ds 
~~~
{: .python}

<br>
<img src="../fig/dsContents.png" width = "800" border = "10">
<br>

> ## Displaying dataset properties
> Try looking up the coordinates (coords), attributes (attrs) and data variables (data_vars) for our existing dataset.
> Look at the output and think about what this tells us about our sample dataset. 
{: .challenge}

### Data Arrays

We have queried the dataset details about our datset's dimensions, coordinates and attributes. Next we will look at the variable data contained within the dataset. In the graphic above, there are two variables (temperature and precipitation). In xarray, these observations get stored as a "data array", which is similar to a conventional array you would find in numpy or matlab. Let's look again at our dataset and extract some data arrays:


Here we see that the "data variables" include "t2m", which is the air temperature at 2 m height. We can isolate the t2m "data cube" as follows: 


~~~
ds.t2m
~~~
{: .python}

This returns a "data array". Note that the associated coordinates and attributes get carried along for the ride. Also note that we are still not reading any data into memory. 

