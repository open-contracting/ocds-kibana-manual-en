# Logstash

Data collection, processing and redistribution engine Logstash is a valuable tool to work with different data sources or decentralized data. We could describe Logstash as a machine that takes data from the indicated locations (collection), it transforms it (processing), and it can later send data to other systems in a new format (redistribution).



![Logstash](../logstash_001.png "Logstash")

These three steps make what is known in Logstash as a Pipeline.

![Logstash Pipeline](../logstash_002.png "Logstash Pipeline")


## Data Collection

Logstash can be configured in a way to observe a folder (or another origin), and when new data files are created or when new records are added to the specified files, this new information will be automatically read and sent to be processed.

This is very useful in systems that create thousands of records per minute (or second) and all this information must be processed and analyzed.

As regards to final data with only one information source with either one or many files, it is also possible to configure Logstash for them to be read just once.

For achieving this, Logstash uses input plugins: [Input Plugins](https://www.elastic.co/guide/en/logstash/current/input-plugins.html, that allow us to read information in many different ways, formats and systems.

## Data Processing

Once Logstash has collected all new data, it is possible to give instructions to Logstash about the data transformations we want to make. For example, we can take data in [CSV](https://en.wikipedia.org/wiki/Comma-separated_values) and turn it into  [JSON](https://en.wikipedia.org/wiki/JSON). We could also take a file with its own non-standardized format and turn it into another one already standardized.



For achieving this, Logstash has at its disposal a set of plugins called "filter":  [Filter plugins](https://www.elastic.co/guide/en/logstash/current/filter-plugins.html). Each filter allows to make a specific transformation and can be used in combinations.

## Redistribution or Data output

Once the Logstash Pipeline has collected and transformed the information, we can indicate what to do with the result. We have many options: we can process new processed data to new files or data can be sent via e-mail, but the most commonly used output is to send this information to an ElasticSearch database.

The official output plugins options are listed in this page: [Output Plugins](https://www.elastic.co/guide/en/logstash/current/output-plugins.html)

---

**Note about Logstash and ElasticSearch** 

Using these two tools together give many benefits, it has to be noted that we can send data directly to ElasticSearch **without** using Logstash.
We can achieve this by using an ElasticSearch feature known as "Ingest" or "Ingest Pipeline", which turns the cluster node into a node specialized in information reception and processing.
However, we will give no further details about this modality, given that it is not used in this manual. Although it is operating, it has limited options and our ElasticSearch cluster takes longer during processing.
