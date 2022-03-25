# PyBer_Analysis
## Overview
### Purpose
The purpose of this anaylsis is to provide V. Isualize a summary DataFrame and a multi-line graphe that shows the total weekly fares for each city type so that they can be used by the decision makers at PyBer. This analysis provides insight on the actual operations of PyBer which will allow the comapny to make strategic decsions in marketing, pricing, and how business varies over time.
### Analysis
#### PyBer Summary DataFrame
In order to properly do this analysis, I imported matplotlib.pyplot and pandas, the necessary dependencies to create the DataFrame and line chart. Once I read in the city_data and ride_data csv, i merged them into a single DataFrame using the .merge() function in pandas. Luckily, they both have the column "city" so the merge was smooth.

    # new_df = pg.merge(left_data_set, right_data_set, how=[‘left’, ‘right’, ‘outer’, ‘inner’-default, ‘cross’], on=["left_column_name","right_column_name"])
    pyber_data_df = pd.merge(ride_data_df, city_data_df, how="left", on=["city", "city"])
   
In order to get the summary DataFrame, I needed to gather information about the data. I needed the total rides for each city type, total drivers for each city type, and the total amount of fares for each city type. Luckily, pandas has the .groupby() function that we can use to group data on a certain column in a DataFrame. I could gather the fare total and ride total from the merged data, however, I couldn't do the same for the drivers. When I used the .groupby() to get the total driver count per city type on the merged dataset, the number of drivers per city is getting multiplied by the number of rides in the city which was really blowing up the results. In order to get the correct number, I had to do the .groupby() on the original city data. Comparing the results below, you can see how using the inflated numbers could have really messed up the analysis.

Incorrect Driver Count      |  Correct Driver Count
:-------------------------:|:-------------------------:
![Incorrect_count](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/incorrect_driver_count.png)|![Correct_count](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/correct_driver_count.png)

Once I got all of the data, I calculated the average fare per ride and average fare per driver and formatted the table to produce the first deliverable.

![PyBer fare summary](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/pyber_summary_df.png)

#### Multiple Line Plot
The second deliverable required is a multiple line plot that shows the total weekly fares for each city type. To create the line graph, I need to orgnize the data into a table such that its rows are weeks, columns are city types, and values are the sum of the fares for that week. The first step was to group the merged dataset by type and date. Luckily, we can use the groupby on multipe paramaters, creating multiple indicies. The code to so is below:

    fares_sum_df = pyber_data_df.groupby(["type","date"]).sum("fare")
    
As you can see, the code is like any other .groupby(), it just holds a list of paramters, that is why there is a set of brackets holding the "type" and "data" paramters. Once we got the table, we needed to use the .pivot() function and set the index to be "date", the columns to be "type", and the values to be "fare". Once we got the table, we just wanted to isolate it to be for the first 4 months of 2019. The first 5 rows of our cleaned up pivot table look like this:

![pivot_df](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/pivot_df.png)

Once I ensured that the index datatype is Datetime, I was able to resample the data to show the data in the weekly breakdwon that was asked. I was able to use the following line of code to produce the table that we can use to build the line chart.

    resampled_sub_fares_sum_df = sub_fares_sum_df.resample('W').sum()

![Resampled_df](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/resampled_df.png)

Using the table above, I was able to create the line chart below using the object-oriented interface method. The code and plot are shown below.
    
    # create the plot and give it a name
    fares_plot = resampled_sub_fares_sum_df.plot(figsize=(20,6))
    # Create labels for the x and y axes.
    fares_plot.set_xlabel("")
    fares_plot.set_ylabel("Fare ($USD)")
    # Create a title
    fares_plot.set_title("Total Fare by City Type")
    # Import the style from Matplotlib.
    from matplotlib import style
    # Use the graph style fivethirtyeight.
    style.use('fivethirtyeight')
    # save figure
    plt.savefig("Analysis/PyBer_fare_summary.png")
    
![PyBer_fare_summary](https://github.com/rmward17/PyBer_Analysis/blob/main/Analysis/PyBer_fare_summary.png)

## Results 
After analyzing the DataFrames and Line Plot, there are clear differences among the different city types. The number of rides, number drivers, and sum of fares decrease from Urban to Rural, however, the average fare per ride and driver increase from Urban to Rural. These differences make sense because there are less drivers the further out you get from urban citites so the cost to ride is higher. Still, there is a large difference in the sum of the fares for the Urban city types vs Rural and Suburban. Conidering that in the Suburban and Rural areas there is more space with less people, residents of those areas are able to have their own means of transportation and don't need to order a car through PyBer as often as those in Urban areas. In a similar vein, the number of drivers is less than the number of rides in the Rural and Suburban cities, however, in the Urban cities that is not the case. There are many more drivers in the Urban cities than there are rides. This makes sense because drivers want to be in the Urban areas because not as many residents have cars and there are more tourists that have taken public transportation to get there. 

## Summary
Based on the results of this analysis, I have the following business reccomendations for consideration:

1. Slightly increase the fare for the suburban and rural fares. Thinking about this in terms of supply and demand, the data shows that there are more drivers than riders in those areas which means the drivers, andPyBer, have more bargining power when it comes to the fares. A light increase wouldn't scare riders away and would generate more revenue in those areas.

2. Put a hold on allowing people to sign up as drivers in Urban cities. Since the number of drivers in Urban cities is grerater than the number of drivers, the power lies with the rider as they have many options and will select the cheapest one. By ceasing the number of drivers that can sign up, the number of rides will increase as the number of drivers will stay the same. Over time, the average fares will increase as will the revenue.

3. Increase marketing and access to PyBer in Suburban and Urban cities. Whether it be a website or a call/text number, if we provide more access and market those different methods to those in Suburban and Rural areas then we can increase the number of riders which will increase the total fares. 

## Conclusion
This analysis, along with the previous one, should be sufficent for V. Isualize to make positive business decisions for PyBer. There are common peaks and valleys across the city types. Those seem to be during holidays and their surrounding weekends. I would want to do the analysis on more data but this could indicate that people use PyBer more often around holidays which is a good opportunity to increase fares since more people are using PyBer.
