# Leveraging Ingest Pipelines
- Elasticsearch is a great data storage and analysis tool. But did you know that it can also do data processing?
- With ingest pipelines, you can leverage data processing capabilities similar to Logstash.

- Create the migrate_accounts Ingest Pipeline

```
GET _cat/indices?v
```


## Create an ingest pipeline called migrate_accounts with the following parameters:
- Removes the account_number field.
- Creates the fullname field, which is a concatenation of the firstname field, a space, and the lastname field.
- Adds a 5% bonus to each account's balance.
- Keeps track of all bonuses that have been applied via the bonus_pct field.

```
PUT _ingest/pipeline/migrate_accounts
{
  "description": "refactoring accounts dataset and adding a bonus five percent",
  "processors": [
    {
      "remove": {
        "field": "account_number"
      }
    },
    {
      "set": {
        "field": "_source.fullname",
        "value": "{{_source.firstname}} {{_source.lastname}}"
      }
    },
    {
      "script": {
        "description": "adds a bonus and increments the bonus counter",
        "source": """
          ctx.balance += ctx.balance * 0.05;
          if (ctx.bonus_pct == null) {
            ctx.bonus_pct = 5;
          } else {
            ctx.bonus_pct += 5;
          }
        """
      }
    }
  ]
}
```

```
GET _ingest/pipeline/migrate_accounts
```

- The migrate_accounts ingest pipeline should display in the response pane.

### Perform a reindex operation that:
- Configures the remote source for the accounts1 node using the 10.0.1.101 IP address, the HTTP interface on port 9200, and the credentials provided for the accounts1 server.
- Copies the accounts index from the accounts1 node to a local index named accounts.
- Processes the copied data with the migrate_accounts ingest pipeline.
```
POST _reindex
{
  "source": {
    "remote": {
      "host": "http://10.0.1.101:9200",
      "username": "elastic",
      "password": "elastic_acg"
    },
    "index": "accounts"
  },
  "dest": {
    "index": "accounts",
    "pipeline": "migrate_accounts"
  }
}
```

```
GET accounts/_search
```

### Scroll through the data returned for the accounts in the response pane and verify that:
- The account_number field has been removed.
- A fullname field exists that displays the first name and last name of the account holder, as concatenated from the firstname and lastname fields.
- A bonus_pct field exists that displays 5, indicating that the bonus has been applied.
- The balance field reflects an amount that includes the 5% bonus.