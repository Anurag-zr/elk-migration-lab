# Installation ES 9.x
Add ES 9.x repo, and install ES 9.x

## 1. Remove old Elastic repos

```bash
sudo rm -f /etc/apt/sources.list.d/elastic-7.x.list \
            /etc/apt/sources.list.d/elastic-8.x.list \
            /etc/apt/sources.list.d/elastic.list
```

## 2. Add Elastic 9.x Apt repo & key

```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch \
  | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg

echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" \
  | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
```

```bash
root@elk-box:/usr/share/keyrings# curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch \
  | sudo gpg --dearmor -o /usr/share/keyrings/elastic.gpg
File '/usr/share/keyrings/elastic.gpg' exists. Overwrite? (y/N) y
root@elk-box:/usr/share/keyrings# echo "deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main" \
  | sudo tee /etc/apt/sources.list.d/elastic-9.x.list
deb [signed-by=/usr/share/keyrings/elastic.gpg] https://artifacts.elastic.co/packages/9.x/apt stable main
```

## 3. Install Elasticsearch

```bash
sudo apt-get update
sudo apt-get install -y elasticsearch
```
```bash
Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : C7BvH4uIGY2Euf68crDQ

If this node should join an existing cluster, you can reconfigure this with
'/usr/share/elasticsearch/bin/elasticsearch-reconfigure-node --enrollment-token <token-here>'
after creating an enrollment token on your existing cluster.

You can complete the following actions at any time:

Reset the password of the elastic built-in superuser with 
'/usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic'.

Generate an enrollment token for Kibana instances with 
 '/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana'.

Generate an enrollment token for Elasticsearch nodes with 
'/usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s node'.
```

## 4. Enable and start ES
```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

## 5. Verify its up 
```bash
curl --cacert /etc/elasticsearch/certs/http_ca.crt \
  -u elastic:<PASSWORD> https://localhost:9200
```

```bash
root@elk-box:/home/anurag# curl --cacert /etc/elasticsearch/certs/http_ca.crt \
  -u elastic:C7BvH4uIGY2Euf68crDQ https://localhost:9200
{
  "name" : "elk-box",
  "cluster_name" : "elasticsearch",
  "cluster_uuid" : "YK-6VwZoSfWJr1P3d1vL7A",
  "version" : {
    "number" : "9.1.3",
    "build_flavor" : "default",
    "build_type" : "deb",
    "build_hash" : "0c781091a2f57de895a73a1391ff8426c0153c8d",
    "build_date" : "2025-08-24T22:05:04.526302670Z",
    "build_snapshot" : false,
    "lucene_version" : "10.2.2",
    "minimum_wire_compatibility_version" : "8.19.0",
    "minimum_index_compatibility_version" : "8.0.0"
  },
  "tagline" : "You Know, for Search"
}
```