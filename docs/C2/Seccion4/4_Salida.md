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
1. Send these documents to ElasticSearch.

For the former, we will use the [Output File Plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-file.html), 
and in the options we should specify the log file's name, which should be created if nonexistent and overwrite all previous information.

To send ElasticSearch the files, we will use another plugin. This plugin has multiple options,but in this case we will focus on three of them. We recommend to check the manual.

[Output ElasticSearch](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html)

These are the options we are going to use:
- `index`: It shows the index name the document will be sent to.
- `hosts`: It indicates the `hostname` of ElasticSearch's server.
- `document_id`: This is a **VERY** important option. It enables Logstash to identify the document with a
  unique ID. Likewise, which in turn will enable ElasticSearch to detect if a document already exists. In this
  case, the OCDS document has a unique ID, named `ocid`.

[Previous](../Section4.md)
