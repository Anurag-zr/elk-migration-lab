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