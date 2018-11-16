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

[Next](2_Input.md)
