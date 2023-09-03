# Searching with Full-Text Queries
- There are a lot of options when crafting an analyzed full-text query in Elasticsearch

## Craft a Query That Answers the Question: How Many Lines from Shakespeare's Plays Reference Love?
```
GET _cat/indices?v
```

### How many lines from Shakespeare's plays reference love?
```
GET shakespeare/_search
{
  "query": {
    "match": {
      "text_entry": "love"
    }
  }
}
```

###In the first hits block, note the value.
###It should be 1857, meaning 1,857 lines reference "love."

## How many lines from Shakespeare's plays reference hate?
```
GET shakespeare/_search
{
  "query": {
    "match": {
      "text_entry": "hate"
    }
  }
}
```


###In the first hits block, note the value.
###It should be 158, meaning 158 lines reference "hate."

## Craft a Query That Answers the Question: What Is the Play and Line Number Where "To Be, or Not to Be" Is Spoken?

### What is the play and line number where "to be, or not to be" is spoken?
```
GET shakespeare/_search
{
  "query": {
    "match_phrase": {
      "text_entry": "to be, or not to be"
    }
  }
}
```

###In the second hits block, note the line_number value.
###It should be 3.1.64.