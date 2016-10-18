---
title: "masking"
teaching: 10
exercises: 5
questions:
- "What is groupby processing and in what cases is it useful for scientific analysis of multidimensional arrays?"
objectives:
- "Learn the concepts of split/apply/combine and experimenting with xarray groupby processing"
keypoints:
- "xarray provides Pandas-like methods for performing data aggregation over defined groupings in the data"
---

## Masking with WHERE:

So far we have used indexing to return subsets of the original. The subset array shape will be different from the original. However, we often want to retain the array shape and mask out some observations. There are applications here in remote sensing, land cover modeling, etc.

Suppose we need to determine which grid cells had temperatures > 20 deg C on June 21, 1984? We will use [where()](http://xray.readthedocs.org/en/stable/indexing.html#masking-with-where) for this selection:

~~~~
ds.sel(time="1984-06-21").t2m.where(subset.t2m > 293.15).plot()
~~~
{: .python}

<br>
<img src="../fig/whereGT20.png" width = "500" border = "10">
<br>

Another common Earth science application is to create land cover masks. Let's use the sea surface temperature field (sst) to build a land and ocean mask. We'll assign land a value of 1, and ocean a value of 2 (arbitrary). Note that the sst field currently has NaN for all land surfaces:

~~~
ds.sst.isel(time=0).plot()
~~~
{: .python}

<br>
<img src="../fig/sst.png" width = "500" border = "10">
<br>

### Buliding the mask:

Here we'll use some lower-level numpy commands to build the mask. The mask number depends on whether the cells are finite or NaN:

~~~
maskOcean = 2 * np.ones((subset.dims['latitude'], subset.dims['longitude'])) * np.isfinite(subset.sst.isel(time=0))  
maskLand = 1 * np.ones((subset.dims['latitude'], subset.dims['longitude'])) * np.isnan(subset.sst.isel(time=0))  
maskArray = maskOcean + maskLand
maskArray.plot()
~~~
{: .python}

<br>
<img src="../fig/sstMsk.png" width = "500" border = "10">
<br>


### Mask as Coordinates
We can keep the mask as a separate array entity, or, if we are using it routinely, there are advantages to [adding it](http://xray.readthedocs.org/en/stable/data-structures.html#dataarray-coordinates) as a coordinate to the `DataArray`:

~~~
ds.coords['mask'] = (('latitude','longitude'),maskArray)
ds
~~~
{: .python}

<br>
<img src="../fig/dsContentsMask.jpg" width = "700" border = "10">
<br>

Now that the mask is integrated into the coordinates, we can easily apply the mask using `where()`. We can integrate this with statistical functions operating on the array:

~~~
with ProgressBar():
   ds.t2m.mean('time').where(ds.mask == 1).plot()
~~~
{: .python}

<br>
<img src="../fig/t2mLand.png" width = "500" border = "10">
<br>

> ## Calculating a climate index 
> Climate scientists commonly calculate mean diferences in sea and land surface
> temperatures. These differences are used as an index and correlated to other earth 
> surface processes, such as ecological change. Using the air temperature dataset,
> calculate the mean annual difference in SST and t2m?
{: .challenge}


 
