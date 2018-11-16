# Visualize

Apart from a strong search engine, Kibana is a powerful data display that allows us to create and save different types of graphics for data analysis.

Once you enter the visualization section, you can see a table with the already saved visualizations, and a blue button with a + symbol. When you click it, many options to work and visualize data will be displayed.  

## Bar Chart

The bar charts are used to compare the same range of data (for example, the total amount) in different instances. In this case, the government departments.  When we click this, a screen as the following will appear, with all blank values.

!["Gráfico Barras"](BChart.png "Bar Chart")

This is the process to replicate the graphic: * **Y-Axis**
  * *Aggregation:* At this stage, we will choose how data will be added to our graphic. By default, we get "Count", but we should choose the "Sum" option to achieve the total values. A new dropdown named "Field" will open, and we will select `awards.value.amount`, in which field we find the contract value.
  * *Custom label:* The graphics's customization field.
  * *Add metric:* The blue button can be used to add another metric to Y-axis. If we wanted to make a grouped bar graph, we should follow this:

* **Buckets / X-Axis** - Although you have two other options to make this graph: **Split Series** and **Split Chart**, you will use the first option.
  * *Aggregation:* It opens a dropdown with many options. Choosing "Terms" brings up a number of fields.
  * *Field*: S*: We must choose the `buyer.name.keyword` option (you will notice there is no option without keyword).
  * *Order by*: You will order by "metric.Sum of awards.value.amount", the value we set initially, although you could order alphabetically or define another order for the database. 
  * *Order*: You will select "Descending" for the higher values to appear first. If you had selected a different option in *Order by*, the options would have been modified. 
  * *Size*: For the values we will show you in the graph, 20 is a reasonable value.
  * *Custom label:* The graphics's customization field.
  
When you have the panel complete, you should click play (button with blue background) and the graph will be displayed on screen. You can modify the different options to analyze in detail. 

## Pie Chart

We can use the pie charts to know each element's weight (contracting procedures) out of the set (all the dataset).

!["Pie chart"](Pchart1.png "Pie chart")

This is the process to replicate the graph:
* **Metrics**
  * *Aggregation:* We will leave "Count" selected

* **Bucket / Splits Slices** given that we are interested in creating the pie's slices instead of creating several pie charts.  
  * *Aggregation:* Terms again
  * *Field*: `tender.procurementMethod.keyword`
  * *Order by*: "Count"
  * *Oder* Descent
  * *Size*: We leave 5 even though there are three. 
  
You click "play" and a graph will be displayed. But once visualized, we want to know which departments have used this dataset more times. Therefore, we need to click the "Add sub-buckets" button that is in the Buckets section. Once there, we need to repeat the process.

* **Bucket / Splits Slices**
  * *Aggregation:* Terms again
  * *Field*: `buyer.name.keyword`
  * *Order by*: "Count"
  * *Oder* Descent
  * *Size*: We leave 5 even though there are three. 

When we click play, this graphic containing the 5 departments that more times have done this kind of contracting in the dataset will be displayed. 

!["Pie char2t"](PChart2.png "Pie Chart2")

Moving on with the analysis, we can pretend we want to change the aggregation. Instead of count, we want it to be the total amount per procedure per department.  
* **Metrics**
  * *Aggregation:* We will select the "Sum" option, and in the new *Field¨* dropdown, we will choose `awards.value.amount`
  
We click play again, which shows us the following graph.

!["Pie Chart 3"](PChart3.png "Pie chart3")

We can continue analyzing, adding and removing values. In order to speed up the process, we have three buttons next to each "Split Slices" text that allow us to "Display/hide", "Change order of appearance" and "Delete slice". Every time we make a change, we need to click the play button. 

## Line Chart

Line charts allow us to show the evolution over time. In this occasion, we will make a graph with two values: count and value. 

!["Line Chart"](LChart.png "Line chart")

* **Metrics**
  * *Aggregation:* We will choose the "Sum" option and `awards.value.amount` in the new *Field* dropdown
  * We will click *Add metric*
  * *Aggregation:* We will leave "Count" selected.
  
* **Buckets / X-Axis** - Although you have two other options to make this graph: **Split Series** and **Split Chart**, you will use the first option.
  * *Aggregation:* This time we will select "Date Histogram" as we want to make a time series.
  *Field:* We will use the  `awards.contractPeriod.endDate`, the date when the contract is awarded.
  * *Interval:* We will select "Monthly" as our dataset contains 3-year data. 
  
If we pressed play now, the graph would just have the green line, and the blue one would be near 0 as the values between them are very different. We should create a second ax of values in the graph for a good display. For this, we should click *Metrics & Axes* (the dropdown located to the left of play button) and in "Value Axis" dropdown, we should choose "New Axis". This will create a new ax we can edit in the following **Y-Axis** box.

If we observe the graphic, we can notice it ends in 2020, when we are still in 2018. We can add a filter to the data in order to amend this, as we do in [Discover](https://manualkibanaocds.readthedocs.io/es/latest/C3/Seccion2.html). 


## Other graphs available

In Kibana, we have many more options to create graphs. All of them with performances very similar to the previously described. Some recommended ones are: 

* **Tables**: It allows us to create new tables and export them to csv.
* **Area**: This is for values accumulated over time.
* **Maps**: This is for geographical values.
* **Networks**: This is to explore the links between different fields.
