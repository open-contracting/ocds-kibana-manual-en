# Logstash data processing

Now that the platform is up and running, we can look in depth into the collection technical details, processing and data index. As we have seen before, this task corresponds to Logstash.

**IMPORTANT:** Everything we will mention next is implemented in the code as a part of Docker containers. It is not necessary to repeat these steps, we just mention this here for a better understanding of the process.

# Preparing OCDS data in packages

## Available and required format

The file obtained from gob.mx is in format [Record Package](http://standard.open-contracting.org/latest/es/schema/record_package/)

> The record package's scheme describes the structure used by the container to publish records.
> The record content is based on the releases scheme. The package contains important metadata.

```
[
    { paquete de registros ocds },
    { paquete de registros ocds },
]
```

This translates into a structure similar to this one:

```
[
    {
        "uri": "..."
        "version": "..."
        ... otros meta datos ...
        "records": [
            { documento ocds },
            { documento ocds },
            ...
        ]
    },
    {
        "uri": "..."
        "version": "..."
        ... otros meta datos ...
        "records": [
            { documento ocds },
            { documento ocds },
            ...
        ]
    }
]
```

In order to work with this document, we will need to turn it into a format where each line of the file
is an OCDS document.
```
{ documento ocds }
{ documento ocds }
```

In this manner, we can process it with Logstash and later send one document at a time to ElasticSearch.

## Converting the format with `jq` tool

An open source and MIT license tool is available to work with JSON files: [jq](https://stedolan.github.io/jq/).

With this tool, we can manipulate the JSON document and turn it into the required format. Once we have installed this tool and have `jq` command available, we can use a command like this:
```
jq -crM ".[].records[]" "archivo.json" > "archivo.ocds_por_linea"
```
> We recommend you check the `jq` file, but now we will explain what this specific command does.

```
jq
    -c = Presents the JSON document compactly
    -r = Presents the JSON document in values without special formats
    -M = ´Monochromatic
    ".[].records[]" = Jq filter
    "archivo.json" = File to be read
    "archivo.ocds_por_linea" = The file created as a result
```
### Jq filter and data structure

The filter is the most important part of this command. We must review carefully the data structure introduced in the original file in order to understand it.
```
[
    {
        "uri": "..."
        "version": "..."
        ... otros meta datos ...
        "records": [
            { documento ocds },
            { documento ocds },
            ...
        ]
    },
    ...
]
```
For the purpose of this project, we are interested in getting each OCDS file separately. According to JSON documents entry, the path to access them would be:
1. Enter each element of root array: `.[]`
1. For each element, we should access the records property: `.records`
1. Get each element of this array: `.records[]`

Putting all instructions together, and in jq filter note, we should get: `.[].records[]`

The files created by this command are compatible with Logstash. Now, we will continue with the pipeline creation, but first we should review some important concepts.


# Basic Concepts for Logstash Pipelines

## Syntax

Logstash Pipelines definitions use a similar language to simplified programming code blocks.

Each filter or plugin is defined by a block:
```
bloque {

}
```

Sometimes these blocks may be empty
```
bloque { }
```

But we will commonly use options and arguments for these blocks, which are defined as:
```
bloque { opcion => valor }
```

We may have different types of option values:

- Text `opcion => "Texto"`
- Numeric `opcion => 123`
- Boolean (True / False) `opcion => true` or `opcion => false`
- Arrays `opcion => [ "Texto", 123, false ]`
    > Arrays are a different sort of sets

## Pipeline

In the file [pipeline.conf](https://github.com/ProjectPODER/ManualKibanaOCDS/blob/master/pipeline/pipeline.conf), we can find the already designed pipeline for this dataset; let's check each of the blocks composing it.

# Input

This component indicates Logstash where and how to read the original data.

```
input {
  stdin {
    codec => "json"
  }
}
```
For this pipeline, we have decided to read the file from the program standard input. Each line received will be treated as a JSON document and stored in memory for the following step.

# Transformation (filter)

This block indicates Logstash what to do with each of the records read from Input module.

```
filter {
  ruby {
    code => '
      event.get("[compiledRelease]").each do |k, v|
        event.set(k, v)
      end
    '
    remove_field => [ "releases", "compiledRelease", "host", "path" ]
  }
}
```

This could be the Pipeline's most complicated process, and also the most interesting and powerful for our tasks.

This block is composed by a set of filters that behave sequentially. In this case, we will just use a filter: Ruby

### [Ruby Filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-ruby.html)

> This filter is more advanced and requires Ruby knowledge.

This section's objective is to take the `compiledRelease` property of each JSON file received and, in turn, read each property that composes it, and copy it over the file root.

**Example**
```
{
  "compiledRelease": {
    "a": "A",
    "bc": [ "B", "C" ],
    "tercero": {
      "a": "3.A",
      "b": "3.B"
    }
  }
}
```
It would be transformed as:
```
{
  "a": "A",
  "bc": [ "B", "C" ],
  "tercero": {
    "a": "3.A",
    "b": "3.B"
  }
}
```
Finally, the `compiledRelease` property is removed; in the same way as  `releases`, `host` and `path`.

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

For the former, we will use the [Output File Plugin](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-file.html),
and in the options we should specify the log file's name, which should be created if nonexistent and overwrite all previous information.

To send ElasticSearch the files, we will use another plugin. This plugin has multiple options, but in this case we will focus on three of them. We recommend to check the manual.

[Output ElasticSearch](https://www.elastic.co/guide/en/logstash/current/plugins-outputs-elasticsearch.html)

These are the options we are going to use:
- `index`: It shows the index name the document will be sent to.
- `hosts`: It indicates the `hostname` of ElasticSearch's server.
- `document_id`: This is a **VERY** important option. It enables Logstash to identify the document with a
  unique ID, which in turn will enable ElasticSearch to detect if a document already exists. In this case, the OCDS document has a unique ID, named `ocid`.

As we could see, a pipeline's creation for processing with Logstash is the codification of a determined logical process. Each dataset may require different processes, but here is where Logstash power lies: it allows us to show the steps concisely and orderly.
