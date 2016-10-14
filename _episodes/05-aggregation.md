---
title: "arithmetic and aggregation"
teaching: 10
exercises: 5
questions:
- "How do I perform simple arithmetic operations on xarray objects?"
- "How do I calculate statistics along a dimension of an xarray object?"
objectives:
- "using dimension names rather than integer axis numbers to perform common statistical arithmetic and aggregation functions"
keypoints:
- "xarray's labeled dimensions enable simplified arithmetic and data aggregation, enabling many powerful shortcuts"
---

### Arithmetic 

xarray takes advantage of labeled dimensions to simplify arithmetic operations on DataArray objects. Suppose we want to plot the difference in air temperature between January 1 in 1979 versus 1980:

~~~
Temperature1 = ds.t2m.sel(time='1979-01-01T06:00:00')
Temperature2 = ds.t2m.sel(time='1980-01-01T06:00:00')
delta = Temperature1 - Temperature2
delta.plot()
~~~
{: .python}

<br>
<img src="../fig/delTemperature.png" width = "600" border = "10">
<br>

Note that the subtraction is automatically vectorized over all array values, as in numpy.

### Aggregation

Aggregration methods can be applied to a Data Array over a specified dimension. Suppose we want to calculate the average June/July/August temperature for a particular year. We'll create a Data Array that slices out those months of data for a particular year:

~~~  
JJA = ds.t2m.sel(time=slice('1979-06-01T06:00:00','1979-09-01T06:00:00')) 
~~~
{: .python}

Now we simply appply the "mean" aggregation method over the time dimension and plot the result:

~~~~
JJA.mean(dim='time').plot()
~~~
{: .python}

<br>
<img src="../fig/JJA.png" width = "600" border = "10">
<br>

> ## Aggregation 
> Using the JJA Data Array created above, calculate the maximum air temperature during the JJA period at each latitude and longitude.
> Plot the result in degrees Celcius as a map. Also, calculate the standard deviation in global air temperature during the JJA period,
> and plot the results as a 1-D time series.
{: .challenge}

