---
title: "plotting"
teaching: 5
exercises: 0
questions:
- "Does xarray have tools for visualizing the data?"
objectives:
- "learn simple methods to visualize subsets of xarray data in 1 or 2-dimensions"
keypoints:
- "xarray has plotting functinality that is a thin wrapper around the Matplotlib library"
- "xarray uses syntax and function names from Matplotlib whenever possible"
---

### Plotting data in 1 dimension

Let's start visualzing some of the data slices we've been working on so far. We will begin by creating a new variable for plotting a 1-dimensional time series:

~~~
timeSeries = ds.t2m.sel(time=slice('1979-01-01T06:00:00','1979-06-01T06:00:00'),latitude=0.0,longitude=0.0)
~~~
{: .python}

xarray has some very simple tools to enable quick visualizations of the data. Try this:

~~~
%matplotlib inline  # optional, if using Jupyter-notebooks 
timeSeries.plot()
~~~
{: .python}

Your plots can be customized using syntax that is very similar to Matplotlib. For example:

~~~
timeSeries.plot.line('b-^')
~~~
{: .python}

See [here] for additional details on customizing your plots.

### Plotting data in 2 dimensions

Since many xarray applications involve geospatial datasets, xarray's plotting extends to maps in 2 dimensions. Let's first select a 2-D subset of our data by choosing a single date and retaining all the latitude and longitude dimensions:

~~~ 
mapData = ds.t2m.sel(time='1979-01-01T06:00:00')
~~~
{: .python}

Note that in the above label-based lookup, we left did not specify the latitude and longitude dimensions, in which case xarray assumes we want to return all elements in those dimensions.

Now, similar to what we did for 1-D plots, simply call do the following to generate a map:

~~~ 
mapData.plot()
~~~
{: .python}

Customization can occur following standard Matplotlib syntax:

~~~ 
mapData.plot(cmap=plt.cm.Blues)
plt.title('ECMWF global air temperature data')
plt.ylabel('latitude')
plt.xlabel('longitude')
plt.tight_layout()
plt.show()
~~~
{: .python}

