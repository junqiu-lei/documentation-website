---
layout: default
title: Text analyzers
parent: Query DSL
nav_order: 41
---


# Optimizing text for searches with text analyzers

OpenSearch applies text analysis during indexing or searching for `text` fields. There is a standard  analyzer that OpenSearch uses by default for text analysis. To optimize unstructured text for search, you can convert it into structured text with our text analyzers.

## Text analyzers

OpenSearch provides several text analyzers to convert your structured text into the format that works best for your searches.

OpenSearch supports the following text analyzers:

1. **Standard analyzer** – Parses strings into terms at word boundaries per the Unicode text segmentation algorithm. It removes most, but not all, punctuation. It converts strings to lowercase. You can remove stop words if you turn on that option, but it does not remove stop words by default.
1. **Simple analyzer** – Converts strings to lowercase and removes non-letter characters when it splits a string into tokens on any non-letter character.
1. **Whitespace analyzer** – Parses strings into terms between each whitespace.
1. **Stop analyzer** – Converts strings to lowercase and removes non-letter characters by splitting strings into tokens at each non-letter character. It also removes stop words (e.g., "but" or "this") from strings.
1. **Keyword analyzer** – Receives a string as input and outputs the entire string as one term.
1. **Pattern analyzer** – Splits strings into terms using regular expressions and supports converting strings to lowercase. It also supports removing stop words.
1. **Language analyzer** – Provides analyzers specific to multiple languages.
1. **Fingerprint analyzer** – Creates a fingerprint to use as a duplicate detector.

The full specialized text analyzers reference is in progress and will be published soon.

{: .note }

## How to use text analyzers

If you want to use a text analyzer, specify the name of the analyzer for the `analyzer` field: standard, simple, whitespace, stop, keyword, pattern, fingerprint, or language.

Each analyzer consists of one tokenizer and zero or more token filters. Different analyzers have different character filters, tokenizers, and token filters. To pre-process the string before the tokenizer is applied, you can use one or more character filters.

#### Example: Specify the standard analyzer in a simple query

```json
 GET _search
{
  "query": {
    "match": {
      "title": "A brief history of Time",
        "analyzer": "standard"
       }
    }
  }
  ```




<!-- This is a list of the 7 individual new pages we need to write
If you want to select one of the text analyzers, see [Text analyzers reference]({{site.url}}{{site.baseurl}}/opensearch/query-dsl/specialized-analyzers).

## Specialized text analyzers

1. Standard analyzer
1. Simple
1. Whitespace
1. Stop
1. Keyword
1. Pattern
1. Language
1. Fingerprint
-->

## Language analyzer

OpenSearch supports the following language values with the `analyzer` option:
arabic, armenian, basque, bengali, brazilian, bulgarian, catalan, czech, danish, dutch, english, estonian, finnish, french, galicia, german, greek, hindi, hungarian, indonesian, irish, italian, latvian, lithuanian, norwegian, persian, portuguese, romanian, russian, sorani, spanish, swedish, turkish, and thai.

To use the analyzer when you map an index, specify the value within your query. For example, to map your index with the French language analyzer, specify the `french` value for the analyzer field:

```json
 "analyzer": "french"
 ```

#### Sample Request

The following query maps an index with the language analyzer set to `french`:

```json
PUT my-index-000001

{
  "mappings": {
    "properties": {
      "text": { 
        "type": "text",
        "fields": {
          "french": { 
            "type":     "text",
            "analyzer": "french"
          }
        }
      }
    }
  }
}
```

<!-- TO do: each of the options needs its own section with an example. Convert table to individual sections, and then give a streamlined list with valid values. -->