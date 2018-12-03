# Management

Kibana's data management panel allows you to modify the imported data properties, reformat the data or create new scripted fields.

In the first «Management» screen, we can manage the «ElasticSearch» database or the visualization in «Kibana». In this manual, we will only approach the «Index Patterns» part, which allows us to either reconfigure the fields' properties or create scripted fields. In order to expand Kibana's management knowledge, we recommend you [check the official manual](https://www.elastic.co/guide/en/kibana/current/management.html).

## Reconfigure a field

When accessing, we will find a table with a search engine that has each index's fields. We can find in each field information about the type, format (only individually), showing if it is searchable, if it can be added (strings ending in *keyword* are aggregated), and if we have excluded it. After each line, you will find an edition symbol, which you can click to reformat.

A typical use for this would be to import an identifier that has only numbers (award IF, postal code, budget lines, etc.), and is identified as a number instead of a string. To modify it, we only need to set the new format and the way we want to visualize it in the «format» part.

## Scripted fields

Kibana allows us to create new fields from calculations and use them permanently in the application. To explain how they work, we will calculate the days passed since the tender publication until the offer reception.

In «Index patterns» screen, we have three tabs: «Fields», «Scripted fields» and «Source Fields». We will click in «Scripted fields», where we have the scripted fields, and later in the button «Add scripted field» to start calculating.

!["Scripted Fields"](ScriptedFields.png "Scripted Fields")

Before start, we should identify the fields we are going to work with. In our case, it will be `tender.awardPeriod.startDate` and `tender.awardPeriod.endDate`, both of them are dates and Kibana recognizes them as dates. The result should be the number of days apart. 

If you follow the image form:

1. **Name:** ID Field. For example:`tender.awardPeriod.duration`, to keep OCDS dataset language.
1. **Language:** Dropdown menu with "painless" and "expression" options. We suggest you work with painless, since it is the syntax we have become familiar in Discover, and will most likely be supported in next versions.
1. **Type:** Dropdown menu to choose the field type we will create. In this case, we are going to use "Number".
1. **Format:** We will define the number as "Duration", and here more dropdowns menus will pop up. In "Input format", we will choose "Milliseconds" (because that is how we defined it in the formula); and in "Output format", we will choose "Days", since we are looking for the amount of days. (Note: the "Human Readable" option makes the queries more difficult). Also, we have a numeric field for decimals. In this case, we are going to use 2.
1. **Popularity:** This numeric field is calculated by Kibana according to use to show the highlighted fields in many screens of the application. If you want it to be highlighted, we suggest setting a high value from the moment you finish. In this case, we will choose 10.
1. **Script:** This is the field where we will make our script.
We suggest you read the [official guide](https://www.elastic.co/guide/en/elasticsearch/reference/6.x/search-request-script-fields.html) to have a deeper understanding. This is the script we will use:
    ```
    (doc['tender.awardPeriod.endDate'].value.getMillis() - doc['tender.awardPeriod.startDate'].value.getMillis())
    ```
    What the script does is bring up Tender Start and End fields, show its values, convert them into milliseconds and subtract the end from the start. To make it usable and work with the rest of the platform, we have formatted it to days with two decimals.

    Analyzing the script's elements in detail:  
    * `doc['Nombre.Campo']` To read a field in our database.  
    * `.value` provides us with the value that we can use for mathematical operations.
    * `.getMillis()` turns the value into milliseconds.
    * ` - ` It is the subtraction operator, other mathematical operators can be used.

    For more details, [read the complete syntax.](https://www.elastic.co/guide/en/elasticsearch/painless/master/painless-api-reference.html)

7. **Create field:** This is the final button that creates the field for the whole application. In case of a script error, a warning message will pop up and it will not be processed in the application.
