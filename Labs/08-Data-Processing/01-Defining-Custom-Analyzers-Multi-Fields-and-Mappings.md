# Defining Custom Analyzers, Multi-Fields, and Mappings
- Elasticsearch has a plethora of built-in analyzers, as well as the ability to automatically detect the data type for each of your fields
- If you want to fine-tune how your fields are analyzed and mapped, then Elasticsearch has you covered. 

## Create the strings_as_keywords Component Template
```
GET _cat/indices?v
```

### Create the strings_as_keywords component template with the following parameters:
- Dynamically maps string fields as the keyword datatype.
- Only indexes the first 256 characters.

```
PUT _component_template/strings_as_keywords
{
  "template": {
    "mappings": {
      "dynamic_templates": [
        {
          "strings_as_keywords": {
            "match_mapping_type": "string",
            "mapping": {
              "type": "keyword",
              "ignore_above": 256
            }
          }
        }
      ]
    }
  }
}
```


```
GET _component_template/strings_as_keywords
```

- The strings_as_keywords component template should display in the response pane.

## Create the social_media_analyzer Component Template
### Create the social_media_analyzer component template with the following parameters:
- Creates a custom analyzer called social_media that uses the classic tokenizer.
- Uses the lowercase filter and a custom english_stop filter that removes English stop words.
- Uses a custom emoticons mapping character filter with the following mappings:
```
:) as happy
:D as laughing
:( as sad
:') as crying
:O as surprised
;) as winking
```

```
PUT _component_template/social_media_analyzer
{
  "template": {
    "settings": {
      "analysis": {
        "analyzer": {
          "social_media": {
            "type": "custom",
            "tokenizer": "classic",
            "char_filter": [ "emoticons" ],
            "filter": [ "lowercase", "english_stop" ]
          }
        },
        "char_filter": {
          "emoticons": {
            "type": "mapping",
            "mappings": [
              ":) => happy",
              ":D => laughing",
              ":( => sad",
              ":'( => crying",
              ":O => surprised",
              ";) => winking"
            ]
          }
        },
        "filter": {
          "english_stop": {
            "type": "stop",
            "stopwords": "_english_"
          }
        }
      }
    }
  }
}
```

```
GET _component_template/social_media_analyzer
```

## Create the twitter_template Index Template
### Create the twitter_template index template with the following parameters:
- Matches all indices that start with "twitter-".
- Composed of the strings_as_keywords and social_media_analyzercomponent templates.
- Maps the tweet field as an analyzed string field with the social_media analyzer.
- Maps a tweet.keyword multi-field as type keyword and only indexes the first 280 characters.
- Sets the number of shards to 1 and replicas to 0.

```
PUT _index_template/twitter_template
{
  "index_patterns": ["twitter-*"],
  "composed_of": ["social_media_analyzer", "strings_as_keywords"],
  "template": {
    "mappings": {
      "properties": {
        "tweet": {
          "type": "text",
          "analyzer": "social_media",
          "fields": {
            "keyword": {
              "type": "keyword",
              "ignore_above": 280
            }
          }
        }
      }
    },
    "settings": {
      "number_of_shards": 1,
      "number_of_replicas": 0
    }
  }
}
```

```
GET _index_template/twitter_template
```


- The twitter_template index template should display in the response pane.