# Input

This component indicates Logstash where and how to read the original data.

```
input {
  stdin {
    codec => "json"
  }
}
```
For this pipeline, we will read the file from the program standard input. Each line the program receives will be treated as a JSON document and stored in memory for the following step.

Transformation or filter: [filter](3_Transformacion.md)
