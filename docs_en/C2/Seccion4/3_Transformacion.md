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
