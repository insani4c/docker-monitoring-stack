# A Docker Monitoring Stack

Docker compose setup of Prometheus, Prometheus SNMP Exporter, Blackbox Exporter, Node Exporter, Promtail, Loki and Grafana.

The SNMP Exporter supports switches and OpenBSD firewalls (if_mib and pf_mib).

Grafana is automatically provisioned with the Prometheus and Loki datasources and a set of dashboards.

## Block external access

To block external access to the Docker container (from the internet), but to allow them from an internal network (for instance, the OpenVPN network), add the following to IPTables:

```bash
# 192.168.1.0/24 is the OpenVPN network address
# enp35s0 is the network interface with the external IP address
iptables -I DOCKER-USER -i enp35s0 ! -s 192.168.1.0/24 -m conntrack --ctdir ORIGINAL -j DROP
```

or in IPTables:

```netfilter
-N DOCKER-USER
-I DOCKER-USER -i enp35s0 ! -s 192.168.1.0/24 -m conntrack --ctdir ORIGINAL -j DROP
```
