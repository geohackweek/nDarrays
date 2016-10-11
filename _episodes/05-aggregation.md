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

### Aggregation


