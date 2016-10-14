---
title: "groupby processing"
teaching: 15
exercises: 5
questions:
- "What is groupby processing and in what cases is it useful for scientific analysis of multidimensional arrays?"
objectives:
- "Learn the concepts of split/apply/combine and experimenting with xarray groupby processing"
keypoints:
- "xarray provides Pandas-like methods for performing data aggregation over defined groupings in the data"
---

## GroupBy processing
* xray has powerful [GroupBy](http://xray.readthedocs.org/en/stable/groupby.html) processing tools
* this is similar to GROUP BY processing in SQL
* in all cases we **split** the data, **apply** a function to independent groups, and **combine** back into a known data structure


## GroupBy processing: example
* we often want to build a time series of change from spatially distributed data
* let's calculate the average air temperature over the globe for the full time period
* remember: split, apply, combine!

### Groupby processing: split

* we can groupby the name of a variable or coordinate
* this returns an xray groupby object

ds.t2m.groupby('time')

### Groupby processing: apply
* when providing a single dimension to the GroupBy command, xray applies the function across the remaining dimensions
* we could do this:


~~~
def mean(x):
    return x.mean()

ds.t2m.groupby('time').apply(mean).plot()
~~~
{: .python}

* however, groupby objects have convenient shortcuts (see next slide)
* but, use this approach if the function is non-standard 

~~~
ds.t2m.groupby('time').mean().plot()
~~~
{: .python}

~~~
ds.t2m.groupby('time.year').mean().plot()
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

