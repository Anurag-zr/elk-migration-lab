# Solution: Compatibility issue during migration from ES 8.x to ES 9.x

## 1: Identify problematic indices
```txt
.ds-ilm-history-5-2025.08.01-000001
.ds-ilm-history-*
.ds-.slm-history-*
.tasks
.async-search
.geoip_databases
.apm-*
```

## 2: Restore only needed indices
```json
POST _snapshot/9x_migration_repo/pre_9x_migration_20250902054505/_restore
{
  "indices": "sample_syslog_fwlogs*,elastalert_status*",
  "include_global_state": false,
  "ignore_unavailable": true,
  "include_aliases": true
}
```
indices → limit to your custom data + monitoring indices

include_global_state: false → avoids restoring old cluster settings/security configs that might conflict in 9.x

ignore_unavailable: true → skips indices that don’t exist in snapshot

include_aliases: true → keeps your aliases like sample_syslog_fwlogs

```json
{
  "accepted": true
}
```

## 3: Recreate Missing ILM Policy

```json
PUT _ilm/policy/sample_syslog_fwlogs_ilm_policy
{
  "policy": {
    "phases": {
      "hot": {
        "min_age": "0ms",
        "actions": {
          "rollover": {
            "max_size": "2gb",
            "max_age": "7d"
          }
        }
      },
      "delete": {
        "min_age": "30d",
        "actions": {
          "delete": {
            "delete_searchable_snapshot": true
          }
        }
      }
    }
  }
}
```

```json 
{
  "acknowledged": true
}
```

## 4. Recreate Missing Index Template

```json
PUT _index_template/sample_syslog_fwlogs_template
{
  "index_patterns": ["sample_syslog_fwlogs-*"],
  "template": {
    "settings": {
      "index.lifecycle.name": "sample_syslog_fwlogs_ilm_policy",
      "index.lifecycle.rollover_alias": "sample_syslog_fwlogs"
    },
    "mappings": {
      "properties": {
        "@timestamp":     { "type": "date" },
        "src_ip":         { "type": "ip" },
        "dst_ip":         { "type": "ip" },
        "src_port":       { "type": "integer" },
        "dst_port":       { "type": "integer" },
        "protocol":       { "type": "keyword" },
        "rule_id":        { "type": "integer" },
        "user":           { "type": "keyword" },
        "app":            { "type": "keyword" },
        "threat_level":   { "type": "keyword" },
        "bytes_sent":     { "type": "long" },
        "bytes_received": { "type": "long" },
        "duration":       { "type": "float" },
        "duration_ms":    { "type": "keyword" },
        "interface":      { "type": "keyword" },
        "event_type":     { "type": "keyword" },
        "hostname":       { "type": "keyword" }
      }
    }
  }
}
```

```josn
{
    "acknowledged": true
}