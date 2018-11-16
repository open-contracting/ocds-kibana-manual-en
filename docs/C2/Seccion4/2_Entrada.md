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

Transformation or filter: [filter](3_Transformation.md)
