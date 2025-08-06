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
