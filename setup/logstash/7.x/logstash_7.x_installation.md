# 📦 Logstash Installation (7.x)

This guide documents the steps to install **Logstash 7.x** on a virtual machine for the purpose of ELK stack simulation and migration testing.

---


---

## 🔧 Prerequisites

- Java 11 (already installed for Elasticsearch)
- Ubuntu 20.04 or later
- Elasticsearch 7.x installed and running

---

## 🛠️ Installation Steps

###  1. Add GPG Key

```bash
wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | \
  sudo gpg --dearmor -o /usr/share/keyrings/elastic-keyring.gpg
```
![GPG Key](screenshot/gpg_key_added.png)

### 2. Add logstash APT Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/elastic-keyring.gpg] \
https://artifacts.elastic.co/packages/7.x/apt stable main" | \
sudo tee /etc/apt/sources.list.d/elastic-7.x.list
```
![logstash apt repository](screenshot/repo_added.png)

### 3. update and install logstash
```bash
sudo apt update
sudo apt install logstash
```
![install logstash](screenshot/logstash_install_success.png)

### 4. Enable and Start Logstash Servic
```bash
sudo systemctl enable logstash
sudo systemctl start logstash
sudo systemctl status logstash
```
![logstash service status](screenshot/logstash_service_status.png)

### 5. Verify Logstash Version
```bash
/usr/share/logstash/bin/logstash --version
```

![logstash version](screenshot/logstash_version_check.png)


