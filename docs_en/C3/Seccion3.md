# Management

Kibana's data management panel allows you to modify the imported data properties, reformat the data or create new scripted fields.

In the first «Management» screen, we can manage the «ElasticSearch» database or the visualization in «Kibana». In this manual, we will only approach the «Index Patterns» part, which allows us to either reconfigure the fields' properties or create scripted fields. In order to expand Kibana's management knowledge, we recommend you [check the official manual](https://www.elastic.co/guide/en/kibana/current/management.html).

## Reconfigure a field

When accessing, we will find a table with a search engine that has each indices' fields. We can find in each field information about the type, format (only individually), showing if it is searchable, if it can be added (strings ending in *keyword* are aggregated), and if we have excluded it. After each line, you will find an edition symbol, which you can click to reformat.

As a case of use, we may import an identifier that only has numbers (award IF, postcode, budget lines, etc.), and it is not identified as a string but as a number. In order to modify them, we should only enter the screen and set a new format in «format» area as well as set how we want it to be visualized.

## Scripted fields

Kibana allows us to create new fields from calculations and use them permanently in the application. To explain how they work, we will calculate the days passed since the tender has been published until the offer reception.

In «Index patterns» screen, we have three tabs: «Fields», «Scripted fields» and «Source Fields». We will click in «Scripted fields», where we have the scripted fields, and later in the button «Add scripted field» to start calculating.

!["Scripted Fields"](ScriptedFields.png "Scripted Fields")

Before starting, we should identify the fields we are going to work with. In our case, it will be `tender.awardPeriod.startDate` and `tender.awardPeriod.endDate` , both of them are dates and Kibana recognizes them as dates. The result should be the number of days apart.  

If you follow the image form:

1. **Name:** The name to identify our field. For example:`tender.awardPeriod.duration` , to go on with the same OCDS dataset language. 
1. **Language:** It is a dropdown with "painless" and "expression" options. We suggest you work with painless, given that it is the syntax we will become familiar in Discover, and which will surely be supported in next versions. 
1. **Type:** Dropdown to choose the field type we will create. In this case, we are going to use "Number".
1. **Format:** We will define the number as "Duration", and here more dropdowns will pop up. "Input format", where we will choose "Milliseconds" (because that is how we defined it in the formula); and the "Output format", such as "Days", given that we are looking for the amount of days. (Note: the option "Human Readable" makes the fields queries difficult). Also, we have a numeric field for decimals. We shall continue with 2 decimals due to author's preferences.
1. **Popularity:** Kibana is calculating this numeric field according to use to show the highlighted fields in many screens of the application. If you want it highlighted, we suggest you set a high value from the moment you finish. In this case, we will choose a 10.
1. **Script:** This is the field where we will make our script. 
We suggest you read the [official guide](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/search-request-script-fields.html) to have a deeper understanding. This is the script we will use:
    ```
    (doc['tender.awardPeriod.endDate'].value.getMillis() - doc['tender.awardPeriod.startDate'].value.getMillis())
    ```
    What the scripts does it to bring the tender access start and end fields, show its values, convert them into milliseconds and subtract the end from the start. To make it a useful result and work in the rest of the platform, we have formatted it to days with two decimals. 

    Analyzing the script's elements in detail:  
    * `doc['Name.Field']` We can name our database's field in this way.  
    * `.value` provides us with the value that we can use for mathematical operations.
    * `.getMillis()` turns the value into milliseconds
    * ` - ` It is the subtraction operator, other mathematical operators can be used.

    For more details, [inquire the complete syntax.](https://www.elastic.co/guide/en/elasticsearch/painless/master/painless-api-reference.html)

7. **Create field:** This is the final button that creates the field for the whole application. In case of a script error, a warning message will pop up and it will not be processed in the application.
