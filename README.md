
# ELK Stack SIEM Simulation & Migration (7.x → 9.x)

This project simulates a **Security Information and Event Management (SIEM)** use case using the **ELK Stack** (Elasticsearch, Logstash, Kibana), with a full migration path from **Elasticsearch 7.x → 9.x**.  
It is designed as a hands-on lab running inside an **Ubuntu VM (UTM on macOS)**, covering not only version upgrades but also realistic workflows like firewall log parsing, index management, snapshot/restore, and alerting.

---

## Objectives
- ✅ Simulate migration from Elasticsearch **7.x → 8.x → 9.x**
- ✅ Reindex data into compatible formats across versions
- ✅ Build **index templates** and **ILM policies** for log management
- ✅ Parse and normalize **firewall logs** (syslog format, ~15+ fields)
- ✅ Visualize logs in **Kibana dashboards**
- ✅ Configure **ElastAlert2** for SOC-style alerting
- ✅ Enable **snapshot & restore** for upgrade safety and disaster recovery
- ✅ Document breaking changes, compatibility fixes, and migration workflow

---

## Tech Stack
- **Virtualization**: UTM (macOS) with Ubuntu Server CLI  
- **Elasticsearch**: 7.17 → 8.x → 9.x  
- **Kibana**: 7.x & 9.x versions  
- **Logstash**: 7.x pipeline for log parsing  
- **ElastAlert2**: Lightweight alerting engine  
- **Backup & Restore**: FS-based snapshot repositories 

## Repository Structure
ELK-MIGRATION-LAB/ <br>
│<br>
├── config/ # Configurations for all components<br>
│ ├── Elastalert2/<br>
│ │ └── config.yaml<br>
│ ├── elasticsearch/<br>
│ │ ├── 7.x/elasticsearch.yml<br>
│ │ ├── 8.x/...<br>
│ │ └── 9.x/...<br>
│ ├── kibana/<br>
│ │ ├── 7.x/kibana.yml<br>
│ │ └── 9.x/kibana.yml<br>
│ └── logstash/7.x/<br>
│ ├── logstash_pipeline_setup.md<br>
│ ├── sample_fwlogs_pipeline.conf<br>
│ └── templates/<br>
│<br>
├── docs/ # Documentation and analysis<br>
│ ├── Kibana_visualized_screenshot/<br>
│ ├── SOC Rules Usecases.md<br>
│ ├── challenges_solutions.md<br>
│ ├── Enable_minimal_security.md<br>
│ ├── ilm_rollover_simulation.md<br>
│ ├── Reindexing_index_ES_8.x_compatibility.md<br>
│ ├── Resolve_compatibility_issue_ES7_in_ES9.md<br>
│ └── Verify_breaking_changes_8.x.md<br>
│<br>
├── Elastalert Alert Rules/ # SOC-like alert definitions<br>
│ ├── high_threat_level.yaml<br>
│ └── potential_data_exfiltration.yaml<br>
│<br>
├── ES backup/ # Snapshot repositories & restore objects<br>
│<br>
├── migration/ # Stepwise migration notes<br>
│ ├── config files/<br>
│ ├── elastic-backups-repo/<br>
│ ├── screenshot/<br>
│ ├── migration_7.x_to_8.x.md<br>
│ ├── migration_8.x_to_9.x.md<br>
│ └── pre_migration.md<br>
│<br>
├── setup/ # Installation notes<br>
│ ├── elastalert2/elastalert_installation.md<br>
│ └── elasticsearch/<br>
│ ├── 7.x/elasticsearch_7.x_installation.md<br>
│ ├── 8.x/...<br>
│ └── 9.x/...<br>
│<br>
├── .env.example # Environment variables (template)<br>
└── README.md<br>

##  Key Features

### Log Ingestion & Parsing
- Ingested **firewall syslog** logs (simulated + real Check Point & Sophos format planned).  
- Designed **Logstash pipelines** with Grok patterns.  
- Normalized ~15+ fields (e.g., `src_ip`, `dst_ip`, `protocol`, `threat_level`).  

### Index Lifecycle Management
- Created **ILM policy** for hot → delete phase:
  - Rollover at `2GB` or `7d`  
  - Delete after `30d`  
- Applied via **index template** with mappings.  

### Migration Workflow
1. **From 7.x → 8.x**:
   - Reindexed indices into compatible format.  
   - Created snapshots (`pre_8x_migration`).  
   - Verified templates, aliases, and ILM policies.  

2. **From 8.x → 9.x**:
   - Registered dedicated snapshot repo (`9x_migration_repo`).  
   - Took `pre_9x_migration` snapshots (with global state).  
   - Installed ES 9.x & Kibana 9.x.  
   - Restored indices, templates, ILM policies.  
   - Verified rollover functionality and Kibana visualization.  

### Security & Monitoring
- Enabled **minimal security** (`https`, built-in users, roles).  
- Dumped and reviewed built-in roles.  
- Configured **ElastAlert2** with rules for:
  - High threat-level activity  
  - Potential data exfiltration  

### Visualization
- Built **Kibana dashboards** for firewall log trends.  
- Verified restored indices via **custom data views**.  
- Tested ILM rollover dry-runs.  

---

## Example Screenshots
- ### Kibana dashboards for firewall logs  
![Discover](/config/logstash/7.x/screenshot/sample_syslog_fwlog_discover.png)
- ### ILM rollover explain output  
![ILM_policy](/config/logstash/7.x/screenshot/sample_syslog_fwlog_ilm_policy.png)
![Index_template](/config/logstash/7.x/screenshot/index_template.png)
![Index_template_setting](/config/logstash/7.x/screenshot/index_template_setting.png)
![simulate_rollover](/docs/Kibana_visualized_screenshot/simulate_rollover_syslog_fwlogs.png)
- ### Snapshot/restore operations  
![all_snapshot](/ES%20backup/screenshot/all_snapshot.png)
![slm_status](/ES%20backup/screenshot/slm_status.png)
![restore_success](/ES%20backup/screenshot/restoration_success.png)
- ### migration steps operations
![Elastalet_other_indices_status](/migration/screenshot/docs_count_2.png)
![full_migration_snapshot](/migration/screenshot/full_migration_snapshot_4.png)
![full_migration_snapshot_result](/migration/screenshot/full_migration_snapshot_result_5.png)
![data_after_restoration](/migration/screenshot/ES9.x_data_after_migration.png)

---

## Challenges & Solutions
- Handling **breaking changes** between 7.x → 8.x → 9.x  
- Restoring with **aliases & ILM policy errors**  
- Fixing **default data view mismatch in Kibana**  
- Ensuring **path.repo** setup for snapshots  

(See [`docs/challenges_solutions.md`](./docs/challenges_solutions.md))

## Next Steps (Future Work)
- Ingest real **Check Point & Sophos firewall logs**  
- Add **Wazuh** for host-based monitoring (separate VM)  
- Automate pipeline setup with Ansible/Terraform  
- Expand SOC use cases (MITRE ATT&CK coverage)

---

## Acknowledgements
This project is a self-contained lab built for **learning & demonstration** purposes.  
It simulates a **real-world migration workflow** while also showcasing SIEM-style log management in ELK.  
