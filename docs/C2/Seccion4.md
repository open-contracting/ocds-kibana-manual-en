# Logstash data processing

Now we have managed to start the platform, we can see in detail the collection technical details, processing and data index. As we have seen before, this task corresponds to Logstash.

**IMPORTANT:** Everything we will mention next is implemented in the code as a part of Docker containers. It is not necessary to repeat these steps, we just mention this here for a better understanding of the process.

# Preparing OCDS data in packages

## Available and required format

The file obtained from gob.mx is in format [Record Package](http://standard.open-contracting.org/latest/es/schema/record_package/)

> The record package's scheme describes the container structure to publish records.
> The record content is based on the releases scheme. The package contains important metadata.

```
[
    { ocds record package },
    { ocds record package },
]
```

This translates into a structure similar to this one:

```
[
    {
        "uri": "..."
        "version": "..."
        ... another metadata ...
        "records": [
            { ocds document },
            { ocds document },
            ...
        ]
    },
    {
        "uri": "..."
        "version": "..."
        ... another metadata ...
        "records": [
            { ocds document },
            { ocds document },
            ...
        ]
    }
]
```

In order to work with this document, we will need to turn it into a format where each line of the file
is an OCDS document.
```
{ ocds document }
{ ocds document }
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
    "file.json" = File to be read
    "file.ocds_per_line" = The file created as a result
```
### Jq filter and data structure

The filter is the most important part of this command. We must review carefully the data structure introduced in the original file in order to understand it.
```
[
    {
        "uri": "..."
        "version": "..."
        ... another metadata ...
        "records": [
            { ocds document },
            { ocds document },
            ...
        ]
    },
    ...
]
```
For the purpose of this project, we are interested in getting each ocds file separately. According to JSON documents entry, the path to access them would be:
1. Enter each element of root array: `.[]`
1. From each element, we should access records property: `.records`
1. Obtain each element of this array: `.records[]`

Putting all instructions together, and in jq filter note, we should get: `.[].records[]`

The files created by this command are adequate to process with Logstash. Now, we will continue with the pipeline creation, but first we should review some important concepts.


# Basic Concepts for Logstash Pipelines

## Syntax

Logstash Pipelines definitions use a similar language to simplified programming code blocks.

Each filter or plugin is defined by a block:
```
block {

}
```

Sometimes these blocks may be empty
```
block { }
```

But we will commonly use options and arguments for these blocks, and this is defined as:
```
block { option => value }
```

We may have different types of option values:

- Text `option => "Text"`
- Numeric `option => 123`
- Boolean (True / False) `option => true` or `option => false`
- Arrays `option => [ "Text", 123, false ]`
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
For this pipeline, we have decided to read the file from the program standard input. According to the text line the program receives, it will be treated as a JSON document and stored in memory for the following step.

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

This block is composed by a set of filters that behave sequentially. In this case, we will just use a filter: ruby

### [Ruby Filter](https://www.elastic.co/guide/en/logstash/current/plugins-filters-ruby.html)

> This filter is more advanced and requires programming knowledge in Ruby language.

This section's objective is to take the `compiledRelease` property of each JSON file received and, in turn, read each property that composes it as well as copy it over the file root.

**Example**
```
{
  "compiledRelease": {
    "a": "A",
    "bc": [ "B", "C" ],
    "third": {
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
  "third": {
    "a": "3.A",
    "b": "3.B"
  }
}
```
At the end, the `compiledRelease` property is removed; in the same way as  `releases`, `host` and `path.

## Important

When this block is over, each JSON line should have been turned into the desired format and will still be stored in the Logstash memory, ready to be sent to the next block: Output [output](4_Salida.md)


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


As we could see, a pipeline's creation for processing with Logstash is the codification of a determined logical process. Each dataset may require different processes, but here is where Logstash power lies: it allows us to show the steps concisely and orderly.