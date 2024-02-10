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
## Configure gRPC on switch
```bash
configure terminal
netconf-yang
telemetry ietf subscription 100
stream yang-push
filter xpath /memory-ios-xe-oper:memory-statistics/memory-statistic
update-policy periodic 1000
encoding encode-kvgpb
source-address 192.168.178.112
receiver ip address 192.168.178.155 57555 protocol grpc-tcp
end
```
### Notes
```bash
show platform software yang-management process
show telemetry ietf subscription 101 brief
show telemetry ietf subscription 101 detail
show telemetry ietf subscription all
```
## Install yangsuite
### download and install
```bash
pip install yangsuite[core]
```
## References
### IOS-XE 17 Config guide
https://www.cisco.com/c/en/us/td/docs/ios-xml/ios/prog/configuration/176/b_176_programmability_cg/m_176_prog_ietf_telemetry.html
### MDT Demo
https://developer.cisco.com/codeexchange/github/repo/jeremycohoe/cisco-ios-xe-mdt/
https://github.com/jeremycohoe/cisco-ios-xe-mdt/blob/master/c9300-grpc-periodic-examples.cfg
### yangsuite
https://developer.cisco.com/docs/yangsuite/#!welcome-to-cisco-yang-suite
https://github.com/CiscoDevNet/yangsuite



