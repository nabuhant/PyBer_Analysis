# PyBer_Analysis
Module5_MatPlotLib

# Overview of the analysis
## Background
As a data analyst at PyBer, a ride-sharing app company, a project is carried out to analyze all the rideshare data from January to early May of 2019 and to create compelling visualizations for the CEO, V. Isualize. The data is merged into one DataFrame where the columns are the city, city type (rural, suburban, & urban), fare, date & time of ride, unique ride id, and driver count in each city.
The data was analysed and using Python's Matplotlib library visualized into:  
**1 -** A scatter plot demonstrating the relationship between the average fare to the total number of rides per city (categorized by city type and sized by the number of drivers per city). Figure is below:
![image1](/analysis/Fig1.png)  
**2 -** The data's measures of central tendency (mean, median, mode) along with the outliers were visualized using box-and-whisker plots. This was done for the number of rides, fares, and the number of drivers, all by city type. An example is below for the ride count date visualization.
![image2](/analysis/Fig2.png)

**3 -** Also pie charts were used to visualize the % of total fares, the % of total rides, and the % of total drivers, all by city type. An example is below for the % of total drivers by city type.
![image3](/analysis/Fig7.png)  

All figures described above can be found in the [analysis folder](/analysis).

 ## Purpose
The first part of the analysis described in the background gave an overview of what occurred between January and early May of 2019 but did not provide any details regarding the monthly happenings. The second part of the analysis breaks down the data into weekly timeframes and uses a multiple-line graph as its visualization. The graphs show the weekly fare in $USD by city type (rural, suburban, & urban) over the four months.


# Results

## PyBer Data Summary DataFrame
A summary Dataframe was created to plot the multiple-line graph. The DataFrame is a summary of the data. It holds the number of total rides, number of total drivers, average fare per ride, and average fare per driver categorized by city type. To obtain this DataFrame the ride data and city data were merged on the common column, city.  
``` python
pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])
```
Then the **groupby()** operation was applied to the city types along with the **sum()** and **count()** functions to find the total values (rides, drivers, fares) and average fares (per ride and driver). Two examples of how they were used are below:

```python
# Total amount of fares for each city type. groupby() and sum() used
fares_by_type = pyber_data_df.groupby(["type"]).sum()["fare"]
fares_by_type # a Series

# Average fare per ride for each city type. groupby() and count() used
total_rides_type = pyber_data_df.groupby(["type"]).count()["ride_id"]
total_rides_type
```

Below is a the final PyBer Summary DataFrame.  
![image4](/analysis/PyBerSummaryDataFrame.PNG)  

## Date (by Week) Indexed DataFrame
Another DataFrame with the date as the index (specifically set to a DateTimeIndex data type) was created using the **pivot()** function, categorized by city type. Then the **loc()** method was used to get the date range from 2019-01-01 to 2019-04-28. Finally using the **resample()** function, the date range was divided into weekly bins between January and May 2019. Using the **sum()** function, the total fare was calculated for every week. The code to do so is below:

```python
# pivot() to set date as index, type as columns, and fare as values
type_date_df = type_date_df.pivot(index = "date", columns = "type", values = "fare")

# loc() to get weekly bins
range_type_date_df = type_date_df.loc['2019-01-01':'2019-04-29']

# pd.to_datetime to change to DateTimeIndex
range_type_date_df.index = pd.to_datetime(range_type_date_df.index)

# DataFrame created using the "resample()" function by week 'W' which holds the sum of the fares for each week, all by city type.
fare_dates_df = fare_dates_df.resample('W').sum()
```
The resulting DataFrame is below:  
![image6](/analysis/PyBerWeeklyFareDataFrame.PNG)  

This DataFrame was used to plot the multiple-line graph (each line is a city type), with the date (by week) on the x-axis and the total fare ($USD) on the y-axis. The graph was plotted using the *object-oriented* interface method and Matplotlib's *fivethirtyeight* graph style.
``` python
#Object-Oriented Interface method
fig = plt.figure(figsize = (22, 6))
ax = fig.add_subplot()
by_week_df.plot(ax=ax)

ax.set_title("Total Fare by City Type")
ax.set_xlabel("")
ax.set_ylabel("Fare ($USD)")
```
The final formatted graph is below:
![image5](/analysis/PyBer_fare_summary.png)

## Result's Analysis
Looking at the summary DataFrame, one can conclude that the rides demand for PyBer is much higher (13 and 2.6 times more for rural and suburban cities, respectively) in urban cities. As a result, the total fares are greater in urban cities. Also, the number of drivers in urban cities is much greater as well (rural is 78 and suburban is 490 while urban is 2,405). Yet, with higher demand in urban cities and a larger number of available drivers (greater supply) the average fare per driver is the lowest in comparison to rural and suburban cities.  
This trend of lower rides and therefore drivers and total fares as a city type becomes smaller (i.e., suburban than rural), could be attributed to the city's size. In smaller cities ride sharing is more likely to be less common in comparison to urban cities, therefore has less demand. Similarly, the trend of having a higher average fare per ride and driver could also be attributed to the size of the city. For an instance, in rural areas ride sharing is more likely to be used by customers when they need to travel a long distance while in urban cities travel distance is shorter.

# Summary
Based on the results, three recommendations to the CEO for addressing any disparities among the city types are:  
**1-** Provide more encouragement through marketing and advertisement campaigns for customers to start tipping drivers, specifically in urban cities.  
**2-** During high demand periods (end of February according to the line graph), notify drivers of urban areas and allow them to operate in rural or suburban areas such that the demand is met in areas with fewer drivers and urban drivers are earning more money.  
**3-** Increase the demand in rural areas by targeting potential customers from that area and offering customers incentives for using the PyBer service.  
