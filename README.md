# ELK Stack Migration (Elasticsearch 7.x → 9.x)

This project demonstrates a simulated migration from Elasticsearch 7.x to 9.x inside a virtualized environment using UTM on macOS. It includes setup, configuration, snapshot/restore, and compatibility fixes.


## 🎯 Objectives
- Understand differences between Elasticsearch 7.x and 9.x
- Simulate production-like upgrade workflow
- Document steps, configs, and gotchas
- Full setup on Ubuntu VM
- Parsing custom firewall logs using Logstash
- Index templates, ILM policies, and Kibana dashboards
- Final migration to Elasticsearch 9.x and validation

## 🧰 Tech Stack
- UTM VM (Ubuntu Server CLI)
- Elasticsearch 7.17 → 9.x
- Kibana
- Logstash 
- Systemd, curl, wget, apt

## 🛠️ Structure
- `setup-scripts/`: Automated install/upgrade scripts
- `config-files/`: Custom config for ES, Kibana, Logstash
- `snapshots/`: Example snapshot & restore steps
- `notes/`: Version diff, errors, observations

## 📸 Screenshots
- [ ] Add system setup screenshots here

## 👨‍💻 Author
Anurag . – DevSecOps enthusiast

