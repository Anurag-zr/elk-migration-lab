# Challenge: Handling Legacy vs Modern Index Templates Before Migraing to ES 9.x
After restoring snapshots from **Elasticsearch 7.x** into **8.x**, both legacy `_template` and new `_index_template` objects exist for the same indices (e.g., `sample_syslog_fwlogs`).  
Leaving legacy `_template` objects around may cause confusion or conflicts during index creation/rollover in newer versions, especially in **Elasticsearch 9.x**, where `_template` is fully deprecated.

# Solution 
We must **safely delete the legacy `_template`** without breaking index creation.  
This involves:
1. Backing up the legacy template.
2. Verifying that the `_index_template` contains everything needed (mappings, settings, ILM).
3. Deleting the `_template`.
4. Testing index creation/rollover.
5. Knowing how to recover if something breaks.

## Backup the legacy _template
```bash
escurl "$ES_URL/_template/sample_syslog_fwlogs?pretty" > legacy_sample_syslog_fwlogs_template.json
```
## Verify what's inside _index_template
```bash
escurl "$ES_URL/_index_template/sample_syslog_fwlogs?pretty" > index_sample_syslog_fwlogs_template.json
```

## Delete the legacy _template
```bash
escurl -X DELETE "$ES_URL/_template/sample_syslog_fwlogs?pretty"
```

## Test nothing breaks

### create a test index (simulates rollover):
```bash
escurl -X PUT "$ES_URL/sample_syslog_fwlogs-test" -H 'Content-Type: application/json' -d '{
  "settings": { "number_of_shards": 1 }
}'
```

### check applied settings/mappings:
```bash
escurl "$ES_URL/sample_syslog_fwlogs-test/_settings?pretty"
escurl "$ES_URL/sample_syslog_fwlogs-test/_mapping?pretty"
```

### do a dry run
```bash
escurl -X POST "$ES_URL/sample_syslog_fwlogs/_rollover?dry_run=true&pretty"
```

## Recovery Plan if something breaks

### recreate the old template
```bash
escurl -X PUT "$ES_URL/_template/sample_syslog_fwlogs" \
  -H 'Content-Type: application/json' \
  -d @legacy_sample_syslog_fwlogs_template.json
```

### delete any bad test indices
```bash
escurl -X DELETE "$ES_URL/sample_syslog_fwlogs-test"
```