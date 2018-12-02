# Preparing OCDS data in packages

## Available and required format

The file from gob.mx is in [Record Package format](http://standard.open-contracting.org/latest/es/schema/record_package/)

> The record package's scheme describes the container structure to publish records.
> Record contents are based on the releases scheme. The package contains important metadata.

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

In order to work with this document, we will need to convert it to a format where each line of the file
is an OCDS document.
```
{ documento ocds }
{ documento ocds }
```

This way, we can process it with Logstash and later send one document at a time to ElasticSearch.

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

The filter is the most important part of this command. To understand it, we need to analize the data structure of the original file.
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
1. For each element, access records property: `.records`
1. Get each element of this array: `.records[]`

Putting all instructions together, and in jq filter note, we should get: `.[].records[]`

The files created by this command are adequate to be processed with Logstash. Now, we will continue with the pipeline creation, but first, let's review some important concepts.

[Next](1_Conceptos.md)
