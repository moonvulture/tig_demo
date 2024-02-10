# Telegraf Influx Grafana Demo
## Telegraf Installation and Configuration
### Add repo
```bash
cat <<EOF | sudo tee /etc/yum.repos.d/influxdb.repo
[influxdb]
name = InfluxData Repository - Stable
baseurl = https://repos.influxdata.com/stable/\$basearch/main
enabled = 1
gpgcheck = 1
gpgkey = https://repos.influxdata.com/influxdata-archive_compat.key
EOF
```
### Install package
```bash
sudo yum install telegraf
```
### Configure telegraf.conf
```ini
[[outputs.prometheus_client]]
  listen = ":9273"
```
