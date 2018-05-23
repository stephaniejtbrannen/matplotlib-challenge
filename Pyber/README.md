

```python
import matplotlib.pyplot as plt
import pandas as pd
import numpy as np
```


```python
#get csv file location
city_data = "./raw_data/city_data.csv"
ride_data = "./raw_data/ride_data.csv"

#read city data
city_df = pd.read_csv(city_data)

# #group by city and type to to get driver count (this will account for duplicates)
# driver_count = city_df.groupby(["city", "type"])["driver_count"].sum()

# #create data frame for grouped city data
# city_grouped = pd.DataFrame({"driver_count" : driver_count}).reset_index()
```


```python
# #read ride data
ride_df = pd.read_csv(ride_data)

# #group by ride city to get ride and ride count and fare average and sum total
total_rides = ride_df.groupby("city")["ride_id"].count()
avg_fare = ride_df.groupby("city")["fare"].mean()
sum_fare = ride_df.groupby("city")["fare"].sum()

# #create dataframe with grouped ride data
ride_grouped = pd.DataFrame({"total_rides": total_rides
                             ,"avg_fare": avg_fare
                             ,"sum_fares":sum_fare}).reset_index()

#merge city and ride data into one dataframe
merge_df = pd.merge(city_df, ride_grouped, how="outer", on="city")

#previous merged city and ride data
merge_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>avg_fare</th>
      <th>sum_fares</th>
      <th>total_rides</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>21.806429</td>
      <td>610.58</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
      <td>25.899615</td>
      <td>673.39</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
      <td>26.169091</td>
      <td>575.72</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
      <td>22.330345</td>
      <td>647.58</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
      <td>21.332609</td>
      <td>490.65</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
#merge city and ride data into one dataframe
# merge_df = pd.merge(city_df, ride_df, how="outer", on="city")

# #group by ride city to get ride and ride count and fare average and sum total, and driver count
# total_rides = merge_df.groupby(["city","type"])["ride_id"].count()
# avg_fare = merge_df.groupby(["city","type"])["fare"].mean()
# sum_fare = merge_df.groupby(["city","type"])["fare"].sum()
# driver_count = merge_df.groupby(["city","type"])["driver_count"].sum()


# # #create dataframe with grouped ride data
# merge_group = pd.DataFrame({"total_rides": total_rides
#                             ,"avg_fare": avg_fare
#                             ,"sum_fares":sum_fare
#                             ,"driver_count": driver_count}).reset_index()
# merge_group.head()
```


```python
# create dataframes for each city type
urban_df = merge_df.loc[(merge_df["type"] == "Urban")]
suburban_df =  merge_df.loc[(merge_df["type"] == "Suburban")]
rural_df = merge_df.loc[(merge_df["type"] == "Rural")]

urban_df.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>city</th>
      <th>driver_count</th>
      <th>type</th>
      <th>avg_fare</th>
      <th>sum_fares</th>
      <th>total_rides</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Kelseyland</td>
      <td>63</td>
      <td>Urban</td>
      <td>21.806429</td>
      <td>610.58</td>
      <td>28</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nguyenbury</td>
      <td>8</td>
      <td>Urban</td>
      <td>25.899615</td>
      <td>673.39</td>
      <td>26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>East Douglas</td>
      <td>12</td>
      <td>Urban</td>
      <td>26.169091</td>
      <td>575.72</td>
      <td>22</td>
    </tr>
    <tr>
      <th>3</th>
      <td>West Dawnfurt</td>
      <td>34</td>
      <td>Urban</td>
      <td>22.330345</td>
      <td>647.58</td>
      <td>29</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rodriguezburgh</td>
      <td>52</td>
      <td>Urban</td>
      <td>21.332609</td>
      <td>490.65</td>
      <td>23</td>
    </tr>
  </tbody>
</table>
</div>




```python
#create x and y axis and size for each city type
xu= urban_df['total_rides']
yu=urban_df['avg_fare']
su=[n*4 for n in urban_df['driver_count']]

xs= suburban_df['total_rides']
ys= suburban_df['avg_fare']
ss=[n*4 for n in suburban_df['driver_count']]

xr= rural_df['total_rides']
yr= rural_df['avg_fare']
sr=[n*4 for n in rural_df['driver_count']]


#plot each city type in a scatter plot
plt.scatter(xu,yu,s=su, c='lightcoral', label = 'Urban', alpha = .65, edgecolor = 'black', linewidth=1)
plt.scatter(xs,ys,s=ss, c='lightblue', label = 'Suburban',alpha = .75, edgecolor = 'black', linewidth=1)
plt.scatter(xr,yr,s=sr, c='gold', label = 'Rural', alpha = .55, edgecolor = 'black', linewidth=1)

#limit the x and y axix
plt.ylim(15, 45)
plt.xlim(0,40)

# format the legend
lgnd = plt.legend(scatterpoints=1, loc='best', numpoints=1, title="City Type"
           , fontsize=8)

# format size markers in legend
lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]

# display plot title and x and y labels
plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')

#display footnote to right of plot
plt.figtext(0.95, 0.5, 'Note:\nCircle size correlates to driver count per city.', horizontalalignment='left') 

#display plot
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_5_1.png)



```python
#import seaborn library
import seaborn as sns
#set seaborn plot to display
sns.set()

#create x and y axis and size for each city type
xu= urban_df['total_rides']
yu=urban_df['avg_fare']
su=[n*4 for n in urban_df['driver_count']]

xs= suburban_df['total_rides']
ys= suburban_df['avg_fare']
ss=[n*4 for n in suburban_df['driver_count']]

xr= rural_df['total_rides']
yr= rural_df['avg_fare']
sr=[n*4 for n in rural_df['driver_count']]


#plot each city type in a scatter plot
plt.scatter(xu,yu,s=su, c='lightcoral', label = 'Urban', edgecolor = 'black', linewidth=1)
plt.scatter(xs,ys,s=ss, c='lightblue', label = 'Suburban', edgecolor = 'black', linewidth=1)
plt.scatter(xr,yr,s=sr, c='gold', label = 'Rural', edgecolor = 'black', linewidth=1)

#limit the x and y axix
plt.ylim(15, 45)
plt.xlim(0,40)

# format the legend
lgnd = plt.legend(scatterpoints=1, loc='best', numpoints=1, title="City Type"
           , fontsize=8)

# format size markers in legend
lgnd.legendHandles[0]._sizes = [30]
lgnd.legendHandles[1]._sizes = [30]
lgnd.legendHandles[2]._sizes = [30]

# display plot title and x and y labels
plt.title("Pyber Ride Sharing Data (2016)")
plt.xlabel('Total Number of Rides (Per City)')
plt.ylabel('Average Fare ($)')

#display footnote to right of plot
plt.figtext(0.95, 0.5, 'Note:\nCircle size correlates to driver count per city.', horizontalalignment='left') 

#display plot
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_6_1.png)



```python
#group fares by type and get percent of total of all fares
sum_fares = merge_df.groupby("type")["sum_fares"].sum()
sum_sum_fares = merge_df.groupby("type")["sum_fares"].sum().sum()
fares_pct = sum_fares/sum_sum_fares

#create data set for grouped data
fares_pie = pd.DataFrame({"Fares" : sum_fares
                          ,"Pct_Fares" : fares_pct})

#preview grouped data
fares_pie.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Fares</th>
      <th>Pct_Fares</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>4255.09</td>
      <td>0.065798</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>20335.69</td>
      <td>0.314458</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>40078.34</td>
      <td>0.619745</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list types of cities for labels
types = ["Rural", "Suburban", "Urban"]

#get percent of total for each city type
pct_fare = fares_pie["Pct_Fares"]

#list pie slice color
colors = ["gold", "lightblue",  "lightcoral" ]

#explode Urban pie slice
explode = (0, 0, 0.2)

#create pie chart
plt.pie(pct_fare, explode=explode, labels=types, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=-45)

#make pie chart axis to make a scaled
plt.axis("scaled")

#create chart title
plt.title("% of Total Fares by City Type")

#display pie chart
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_8_1.png)



```python
#group merge data by type to get total_rides and percent of total of all rides
sum_ride = merge_df.groupby("type")["total_rides"].sum()
sum_sum_rides = merge_df.groupby("type")["total_rides"].sum().sum()
rides_pct = sum_ride/sum_sum_rides

#create dataframe for grouped data
rides_pie = pd.DataFrame({" Total Rides" : sum_ride
                          ,"Pct Rides" : rides_pct})

#preview grouped data
rides_pie.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Rides</th>
      <th>Pct Rides</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>125</td>
      <td>0.051932</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>657</td>
      <td>0.272954</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>1625</td>
      <td>0.675114</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list types of cities for labels
types = ["Rural", "Suburban", "Urban"]

#get percent of total for each city type
rides_pct = rides_pie["Pct Rides"]

#list pie slice color
colors = ["gold", "lightblue",  "lightcoral" ]

#explode Urban pie slice
explode = (0, 0, 0.5)

#plot and format pie chart 
plt.pie(pct_fare, explode=explode, labels=types, colors=colors,
        autopct="%1.1f%%", shadow=False, startangle=140)

#make axis equal
plt.axis("equal")

#create title
plt.title("% of Total Rides by City Type")

#diplay pie chart
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_10_1.png)



```python
#group merge data by type to get driver count and percent of total of all rides
sum_drivers = merge_df.groupby("type")["driver_count"].sum()
sum_sum_drivers = merge_df.groupby("type")["driver_count"].sum().sum()
drivers_pct = sum_drivers/sum_sum_drivers

#create dataframe for grouped data
drivers_pie = pd.DataFrame({" Total Drivers" : sum_drivers
                          ,"Pct Drivers" : drivers_pct})
#get preview
drivers_pie.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Drivers</th>
      <th>Pct Drivers</th>
    </tr>
    <tr>
      <th>type</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rural</th>
      <td>104</td>
      <td>0.031054</td>
    </tr>
    <tr>
      <th>Suburban</th>
      <td>638</td>
      <td>0.190505</td>
    </tr>
    <tr>
      <th>Urban</th>
      <td>2607</td>
      <td>0.778441</td>
    </tr>
  </tbody>
</table>
</div>




```python
#list types of cities for labels
types = ["Rural", "Suburban", "Urban"]

#get percent of total for each city type
drivers_pct = drivers_pie["Pct Drivers"]

#list pie slice color
colors = ["gold", "lightblue",  "lightcoral" ]

#explode Urban pie slice
explode = (0, 0, 0.25)

#plot and format pie chart 
plt.pie(drivers_pct, explode=explode, labels=types, colors=colors,
        autopct="%1.1f%%", shadow=True, startangle=145)

#plot equal axis
plt.axis("equal")

#create title for pie chart
plt.title("% of Total Drivers by City Type")

#display pie chart
plt.show
```




    <function matplotlib.pyplot.show>




![png](output_12_1.png)

