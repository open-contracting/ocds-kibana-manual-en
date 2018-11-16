# Output

This section indicates Logstash what to do with the new documents. In our case, we want the results to be sent to ElasticSearch.

```
output {
  file {
    path => "/logs/sfp-compranet-ocds-pipeline.log"
    create_if_deleted => true
    write_behavior => "overwrite"
  }
  elasticsearch {
    index => "${ES_INDEX}"
    hosts => ["${ES_HOST}"]
    document_id => "%{ocid}"
  }
}

```

Here, we will proceed with two steps:
1. Save all processed documents in a log file, one per line.
1. Send these documents to ElasticSearch

For the former, we will use the [Output File](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-file.html) Plugin, 
and in the options we should specify the log file's name, which should be created if nonexistent and overwrite all previous information.

We use another plugin with multiple options to send ElasticSearch the files. 
We will specify three of them, but we recommend to check the manual.

[Output ElasticSearch](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html)

These are the options we are going to use:
- `index`: It shows the index name we will send the document to.
- `hosts`: It indicates the `hostname` of the ElasticSearch server.
- `document_id`: This is a **VERY** important option, given that it enables Logstash to identify the document with a
  unique ID. Likewise, it enables ElasticSearch to recognize when a document already exists. In this
  case, the OCDS document has a unique ID, named `ocid`.


[Previous](../Section4.md)
