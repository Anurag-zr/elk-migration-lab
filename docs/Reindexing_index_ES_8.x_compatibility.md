# Reindexing sample_syslog_fwlogs Indices for Elasticsearch 8.x Compatibility

The deprecation API showed that all sample_syslog_fwlogs-* indices required reindexing.

## Check Depreciation warning
```bash
escurl "$ES_URL/_migration/deprecations?pretty"
```

```json
"sample_syslog_fwlogs-2025.08.04-000002" : [
  {
    "level" : "critical",
    "message" : "Old index with a compatibility version < 8.0",
    "details" : "This index has version: 7.17.29",
    "_meta" : {
      "reindex_required" : true
    }
  }
]
```

## Identify Alias and Backing Indices
```bash
escurl "$ES_URL/_cat/indices/sample_syslog_fwlogs-*?v"
escurl "$ES_URL/_cat/aliases/sample_syslog_fwlogs?v"
```
```bash
root@elk-box:/etc/elasticsearch/certs# escurl "$ES_URL/_cat/indices/sample_syslog_fwlogs?pretty"
yellow open sample_syslog_fwlogs-2025.08.14-000003 lf2U6KcHQUafKrPGHdDyVw 1 1   0 0    249b    249b    249b
yellow open sample_syslog_fwlogs-2025.08.04-000002 KHNmpKK_QPWLayDpiOIdJQ 1 1 500 0 228.9kb 228.9kb 228.9kb
```

```bash
root@elk-box:/etc/elasticsearch/certs# escurl "$ES_URL/_cat/aliases/sample_syslog_fwlogs*?pretty"
sample_syslog_fwlogs sample_syslog_fwlogs-2025.08.14-000003 - - - true
sample_syslog_fwlogs sample_syslog_fwlogs-2025.08.04-000002 - - - false
```


## Create New Index with ILM + Template
Manually create a new empty index using your existing index template and ILM policy:

```bash
escurl -X PUT "$ES_URL/sample_syslog_fwlogs-2025.08.20" -H 'Content-Type: application/json' -d '{
  "aliases": {
    "sample_syslog_fwlogs": {}
  }
}'
```
Name it with a timestamp (-2025.08.20) or rollover-like pattern.

Attaches the alias sample_syslog_fwlogs.

Automatically inherits mappings/settings from sample_syslog_fwlogs_template

```bash
root@elk-box:/etc/elasticsearch/certs# escurl -X PUT "$ES_URL/sample_syslog_fwlogs-2025.08.30" -H 'Content-Type: application/json' -d 
'{
  "aliases": {
    "sample_syslog_fwlogs": {}
  }
}'
{"acknowledged":true,"shards_acknowledged":true,"index":"sample_syslog_fwlogs-2025.08.30"}
```

## Reindex Old Index into New Index

```bash
escurl -X POST "$ES_URL/_reindex?pretty" -H 'Content-Type: application/json' -d '{
  "source": {
    "index": "sample_syslog_fwlogs-2025.08.04-000002"
  },
  "dest": {
    "index": "sample_syslog_fwlogs-2025.08.30"
  }
}'
```
```json
{
  "took" : 318,
  "timed_out" : false,
  "total" : 500,
  "updated" : 0,
  "created" : 500,
  "deleted" : 0,
  "batches" : 1,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}
```

**i tried to reindex other index into same dest index to check is it possible and writable**
```bash
root@elk-box:/etc/elasticsearch/certs# escurl -X POST "$ES_URL/_reindex?pretty" -H 'Content-Type: application/json' -d '{
  "source": {
    "index": "sample_syslog_fwlogs-2025.08.14-000003"
  },
  "dest": {
    "index": "sample_syslog_fwlogs-2025.08.30-000001"
  }
}'
{
  "took" : 6,
  "timed_out" : false,
  "total" : 0,
  "updated" : 0,
  "created" : 0,
  "deleted" : 0,
  "batches" : 0,
  "version_conflicts" : 0,
  "noops" : 0,
  "retries" : {
    "bulk" : 0,
    "search" : 0
  },
  "throttled_millis" : 0,
  "requests_per_second" : -1.0,
  "throttled_until_millis" : 0,
  "failures" : [ ]
}
```
the reindexing into sample_syslog_fwlogs-2025.08.30, but since you manually created that index instead of via ILM rollover, the alias entry was added without the is_write_index: true flag.

That means new writes will still go to the 000003 index, not the reindexed 2025.08.30.

So yes — without fixing this, rollover and ILM will not target your reindexed index.

### The fix
```bash
escurl -X POST "$ES_URL/_aliases?pretty" -H 'Content-Type: application/json' -d '{
  "actions": [
    { "remove": { "index": "sample_syslog_fwlogs-2025.08.14-000003", "alias": "sample_syslog_fwlogs" }},
    { "add":    { "index": "sample_syslog_fwlogs-2025.08.30-000001", "alias": "sample_syslog_fwlogs", "is_write_index": true }}
  ]
}'
```
```bash
{
  "acknowledged" : true,
  "errors" : false
}
```

```bash
root@elk-box:/etc/elasticsearch/certs# escurl "$ES_URL/_alias/sample_syslog_fwlogs?pretty"
{
  "sample_syslog_fwlogs-2025.08.30" : {
    "aliases" : {
      "sample_syslog_fwlogs" : {
        "is_write_index" : true
      }
    }
  },
  "sample_syslog_fwlogs-2025.08.04-000002" : {
    "aliases" : {
      "sample_syslog_fwlogs" : {
        "is_write_index" : false
      }
    }
  }
}
```

### dry run index rollover simulation 
```bash
escurl -X POST "$ES_URL/sample_syslog_fwlogs/_rollover?dry_run=true&pretty"
```
```bash
root@elk-box:/etc/elasticsearch/certs# escurl -X POST "$ES_URL/sample_syslog_fwlogs/_rollover?dry_run=true&pretty"
{
  "acknowledged" : false,
  "shards_acknowledged" : false,
  "old_index" : "sample_syslog_fwlogs-2025.08.30-000001",
  "new_index" : "sample_syslog_fwlogs-2025.08.30-000002",
  "rolled_over" : false,
  "dry_run" : true,
  "lazy" : false,
  "conditions" : { }
}
```

## Verify ILM and Alias

```bash
escurl "$ES_URL/sample_syslog_fwlogs-2025.08.30*/_ilm/explain?pretty"
```

```bash
root@elk-box:/etc/elasticsearch/certs# escurl "$ES_URL/sample_syslog_fwlogs-2025.08.30*/_ilm/explain?pretty"
{
  "indices" : {
    "sample_syslog_fwlogs-2025.08.30-000001" : {
      "index" : "sample_syslog_fwlogs-2025.08.30-000001",
      "managed" : true,
      "policy" : "sample_syslog_fwlogs_ilm_policy",
      "index_creation_date_millis" : 1756567918778,
      "time_since_index_creation" : "16.94h",
      "lifecycle_date_millis" : 1756567918778,
      "age" : "16.94h",
      "phase" : "hot",
      "phase_time_millis" : 1756567918829,
      "action" : "rollover",
      "action_time_millis" : 1756567918829,
      "step" : "check-rollover-ready",
      "step_time_millis" : 1756567918829,
      "phase_execution" : {
        "policy" : "sample_syslog_fwlogs_ilm_policy",
        "phase_definition" : {
          "min_age" : "0ms",
          "actions" : {
            "rollover" : {
              "max_age" : "7d",
              "max_primary_shard_docs" : 200000000,
              "min_docs" : 1,
              "max_size" : "2gb"
            }
          }
        },
        "version" : 1,
        "modified_date_in_millis" : 1754206561499
      },
      "skip" : false
    }
  }
}
```

check alias assignment

```bash
escurl "$ES_URL/_cat/aliases/sample_syslog_fwlogs?v"
```

```bash
root@elk-box:/etc/elasticsearch/certs# escurl "$ES_URL/_cat/aliases/sample_syslog_fwlogs?v"
alias                index                                  filter routing.index routing.search is_write_index
sample_syslog_fwlogs sample_syslog_fwlogs-2025.08.30-000001 -      -             -              true
sample_syslog_fwlogs sample_syslog_fwlogs-2025.08.04-000002 -      -             -              false
```

## Clean up Old Indices
```bash
escurl -X DELETE "$ES_URL/sample_syslog_fwlogs-2025.08.04-000002"
```
```bash
root@elk-box:/etc/elasticsearch/certs# escurl -X DELETE "$ES_URL/sample_syslog_fwlogs-2025.08.04-000002"
{"acknowledged":true}
```