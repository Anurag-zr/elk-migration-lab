# ⚙️ Challenges Faced and How I Overcame Them

This document captures some of the technical challenges I encountered during the ELK Stack migration simulation project and how I resolved them.

---

## 🐛 1. Elasticsearch Failed to Restart After Config Change

**Problem:**  
After adding `path.repo` for snapshot setup, Elasticsearch failed to restart with YAML parsing errors.

**Cause:**  
There was a syntax error in `elasticsearch.yml` — incorrect quote format and indentation.

**Solution:**  
- checked elasticsearch: identified root cause
```bash
sudo journalctl -u elasticsearch.service --since "5 minutes ago"
sudo less /var/log/elasticsearch/elasticsearch.log


```
- Carefully validated YAML syntax (using `yamllint` online).
- Replaced fancy quotes with plain double quotes.
- Verified indentation.
- Restarted service successfully with `sudo systemctl restart elasticsearch`.

---

## 📁 2. Folder Permission Issues for Snapshot Repository

**Problem:**  
SLM policy failed to create snapshots due to permission errors.

**Cause:**  
Elasticsearch process didn’t have write access to `/mnt/elastic-backups`.

**Solution:**  
Ran the following command to fix ownership:
```bash
sudo chown -R elasticsearch:elasticsearch /mnt/elastic-backups
```

## 3. Scheduling time in SLM policy

```sql
┌──────────── second (0 - 59)
│ ┌────────── minute (0 - 59)
│ │ ┌──────── hour (0 - 23)
│ │ │ ┌────── day of month (1 - 31)
│ │ │ │ ┌──── month (1 - 12 or JAN-DEC)
│ │ │ │ │ ┌── day of week (1 - 7 or SUN-SAT)
│ │ │ │ │ │
│ │ │ │ │ │
0 30 1 * * ?
```
## 4. Kibana Alerting Not Working Due to Missing Encryption Keys
**problem**
Kibana showed error:
Additional setup required. You must configure an encryption key to use Alerting.
**Cause**
Kibana requires encryption keys to securely store sensitive information for alerting. These were missing from kibana.yml.
**Solution**
Added the following keys to /etc/kibana/kibana.yml:
```yaml
xpack.encryptedSavedObjects.encryptionKey: "32+character_random_string"
xpack.security.encryptionKey: "another_secure_key_32+chars"
xpack.reporting.encryptionKey: "yet_another_secure_key_32+chars"
```
restart kibana
```bash
sudo systemctl restart kibana
```

## 5. Alert Creation Limitations in Kibana (Basic License)
**Problem:**
Unable to configure alert actions like email, Slack, or other external notifications for SOC use cases in Kibana's rule creation UI.

**Cause:**
Kibana’s Alerting and Actions framework restricts several connectors (Email, Slack, Jira, PagerDuty, etc.) to the Enterprise license. With the Basic (free) license, only limited connectors like webhook, index, and server log are available — which limits real-world alerting capabilities in a SOC simulation.

**Solution:**
To overcome this limitation while staying within open-source and free tools, I opted to use ElastAlert 2 — a community-maintained alerting framework that:

Supports Elasticsearch 7.x to 9.x

Allows rich alerting logic via YAML

Provides native connectors like email, Slack, webhook, etc.

Can run as a background service and integrates well with parsed log indices

This decision enables advanced alerting without upgrading to the Enterprise tier.

## 6. ElastAlert2 Python Version Compatibility Issue
**Problem:**
ElastAlert2 installation failed with an error about Python version incompatibility.

**Cause:**
Latest ElastAlert2 versions (≥ 2.9.0) require Python 3.12+, while the lab environment uses Python 3.10.

**Solution:**
Checked available Git tags and installed a compatible version:

```bash
# Go to elastalert2 directory
cd /home/anurag/elk-lab/elastalert2

# Checkout version compatible with Python 3.10
git checkout 2.8.0

# Install within virtual environment
pip install .
```

## 7. While Creating ElastAlert Rule - Field Mapping & Keyword Usage
**problem**
Our first attempts with should queries returned zero matches even though Kibana showed hits.

**cause**
This happened because we queried threat_level.keyword instead of the correctly mapped threat_level field.

**solution**
verify field names and their data types using:
```http
GET sample_syslog_fwlogs-*/_mapping
```

## 8. While Creating ElastAlert Rule - Filter Syntax Confusion (should vs terms)
**problem**
The should construct didn’t return expected results, while the terms filter worked.

**cause**
Our original data mapping + ingestion process meant that not all documents had the .keyword subfield populated, making term matching fail in should queries.

**solution**
try different filter and read documentation
```yaml
filter:
  - terms:
      threat_level: ["high", "critical"]
```

## 9. Handling Legacy vs Modern Index Templates Before Migrating to ES 9.x
**Problem**

After restoring snapshots from **Elasticsearch 7.x** into **8.x**, both legacy `_template` and new `_index_template` objects exist for the same indices (e.g., `sample_syslog_fwlogs`).  
Leaving legacy `_template` objects around may cause confusion or conflicts during index creation/rollover in newer versions, especially in **Elasticsearch 9.x**, where `_template` is fully deprecated.

**Why it matters**
- **Legacy `_template`** objects may override or conflict with `_index_template`.
- Migration to **9.x** could fail or apply incorrect mappings/ILM policies.
- Cleaning them up ensures the cluster is aligned with modern standards.

**Solution**
[Verify_breaking_changes_8.x](./Verify_breaking_changes_8.x.md)
Migration path to **Elasticsearch 9.x** becomes clean and forward-compatible.


## 10. Compatibility issue during migration from ES 8.x to ES 9.x
**Problem**

During restoring indexes in ES 9.x, find out some indices and settings is not compatible with this version. These are:
       ".ds-.slm-history-7-2025.08.30-000003",
        ".ds-ilm-history-5-2025.08.31-000002",
        ".ds-ilm-history-7-2025.08.21-000002",       
        ".ds-.slm-history-7-2025.08.16-000001",
        ".ds-.slm-history-7-2025.08.23-000002",
        ".kibana-event-log-7.17.29-000002",
        ".kibana_task_manager_7.17.29_001",
        ".kibana-event-log-7.17.29-000001",
        ".ds-.slm-history-5-2025.08.06-000001" etc

**cause**
Some system indices (like .ds-ilm-history-*, .slm-history-*, .tasks) were created in 7.x compatibility mode.

In ES 9.x, they cannot be upgraded directly unless they were marked read-only before snapshot.

Since the snapshot was already taken, the workaround is to exclude those problematic indices from restore (they’ll be auto-recreated fresh by 9.x).

**why it matters**
- without navigating through this problem, we can't restore our index data and ES 9.x keep throwing error.

**Solution**
- Find attached file for detail solution
[Resolve_comp_issue_ES7_in_ES9](./Resolve_compatibility_issue_ES7_in_ES9.md)