# Kibana

Although ElasticSearch and Logstash can be queried or invoked using a tailored developed software and commands in the server to make queries to the indices, it is important to make these databases available to every kind of user. This is why Kibana is important.

Kibana is the user graphic interface of ELK platform, and it allows to make analysis, queries and visualizations of different types of data in our indices.

## Features

### Discover data

One of Kibana's most basic features is to explore data, document by document, or through specialized queries.

!["Discover data with Kibana"](../kibana_004.png "Explore data with Kibana")

Later we will learn more about the language ([Query DSL](https://www.elastic.co/guide/en/elasticsearch/reference/6.4/query-dsl.html)) that Kibana and ElasticSearch use to build more advanced queries.


## Visualize data

In this function, we can create graphs, maps and other type of visualizations with data in our indices.

> To make maps, our documents should contain information such as geographical coordinates. Our documents should contain numeric data to make graphs with sums or totals.

!["Visualizing data with Kibana"](../kibana_005.png "Visualizing data with Kibana")


## Create and share dashboards

This function is popular when we have already defined what information we want to obtain from our indices and how we want to visualize it.

A Dashboard is a set of visualizations that are updated in real time and that can be shared to users outside the platform.


### Development console and other functions

Among other tools, Kibana also allows us to make manual queries over our clusters and indices by using a developer's console. This is an advanced feature that can be used to fix errors.

Also, depending on the configurations, it can show a user admin dashboard, indices and documents. We can also monitor completely the appropriate operation of our ELK platform. 

Some advanced features require buying a license from Elastic.co. In this manual, we will only work with free features available in the ELK open source platform. 

## More examples

!["Kibana Interface"](../kibana_001.jpg "Kibana Interface")

!["Interfaz Kibana"](../kibana_002.jpg "Kibana Interface")

!["Interfaz Kibana"](../kibana_003.jpg "Kibana Interface")

