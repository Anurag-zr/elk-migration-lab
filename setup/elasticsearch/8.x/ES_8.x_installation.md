# Install Elasticsearch 8.x (temporary hop)
Add Elastic 8.x repo, install ES 8.x only

```bash
curl -fsSL https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
echo "deb https://artifacts.elastic.co/packages/8.x/apt stable main" | sudo tee /etc/apt/sources.list.d/elastic-8.x.list
sudo apt-get update
sudo apt-get install -y elasticsearch
```

![ES_8.x_apt_installation](./screenshot/ES_8.x_apt_installation.png)

some important steps 

![ES_8_security_configuration](./screenshot/ES_8.x_security_configuration.png)

```bash
Authentication and authorization are enabled.
TLS for the transport and HTTP layers is enabled and configured.

The generated password for the elastic built-in superuser is : Q+s3nlujemkc-KtS2=6T

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

### configure path.repo ( so ES 8 can see your 7.x snapshot)
```bash
sudo nano /etc/elasticsearch/elasticsearch.yml
# add/ensure:
path.repo: ["/mnt/elastic-backups"]
```
[ES_8.x_config](/config/elasticsearch/8.x/elasticsearch.yml)

### start ES 8.x

```bash
sudo systemctl daemon-reload
sudo systemctl enable elasticsearch
sudo systemctl start elasticsearch
```

Security is enabled by default in 8+. Grab/reset the elastic password and create a Kibana token if you’re installing Kibana 8:

```bash
sudo /usr/share/elasticsearch/bin/elasticsearch-reset-password -u elastic
sudo /usr/share/elasticsearch/bin/elasticsearch-create-enrollment-token -s kibana
```