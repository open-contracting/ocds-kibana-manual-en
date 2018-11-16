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
jq -crM ".[].records[]" "file.json" > "file.ocds_per_line"
```
> We recommend you check the `jq` file, but now we will explain what this specific command does.

```
jq
    -c = Presents the JSON document compactly
    -r = Presents the JSON document in values without special formats
    -M = ´Monochromatic
    ".[].records[]" = Jq filter
    "file.json" = File to be read
    "filer.ocds_por_linea" = The file created as a result
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
For the purpose of this project, we are interested in getting each OCDS file separately. According to JSON documents entry, the path to access them would be:
1. Enter each element of root array: `.[]`
1. From each element, we should access records property: `.records`
1. Obtain each element of this array: `.records[]`

Putting all instructions together, and in jq filter note, we should get: `.[].records[]`

The files created by this command are adequate to process with Logstash. Now, we will continue with the pipeline creation, but first we should review some important concepts.

[Next](1_Conceptos.md)
