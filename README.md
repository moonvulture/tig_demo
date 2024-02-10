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
### Configure etc/telegraf/telegraf.conf
```ini
# inputs for gRPC
[[inputs.cisco_telemetry_mdt]]
  transport = "grpc"
  service_address = ":57555"

# outputs for prometheus
[[outputs.prometheus_client]]
  listen = ":9273"

# outputs for influx
[[outputs.influxdb_v2]]
  urls = ["http://3.68.158.124:8086"]
  token = "P-kpWxtJeJwftzEBZiOiaiZdtL1AGl10JBNxfsfmHT1JzI89XXcM8hy1quhzilIQcs4TuceBigqiXhleJ3GULw=="
  organization = "superlab"
  bucket = "MDT"
```
### Notes
Test configuration file
```bash
telegraf --config /etc/telegraf/telegraf.conf --test
```
Test snmp walk
```bash
snmpwalk -v2c -c public 192.168.178.11 .1.3.6.1.4.1.9.2.1.58
```
Curl metrics
```bash
curl http://localhost:9273/metrics
```
