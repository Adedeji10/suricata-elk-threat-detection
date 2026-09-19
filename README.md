**Suricata IDS \& ELK Stack Threat Detection Pipeline**

A home-lab SOC-style project: deploying Suricata IDS on a hardened Ubuntu host, shipping alerts across a network to a Dockerized ELK stack, and visualizing detections in Kibana.

**Architecture**

Ubuntu VM (Suricata IDS + Filebeat) 

&#x20;   --> network --> 

Windows Host (Docker: Logstash + Elasticsearch + Kibana)

**What This Project Demonstrates**

\- Installing and configuring Suricata IDS with the Emerging Threats Open ruleset

\- Writing a custom threshold-based detection rule for port scan behavior

\- Building a Filebeat -> Logstash -> Elasticsearch -> Kibana pipeline across two machines

\- Real troubleshooting: YAML config errors, rule-file path mismatches, and &#x20; loopback interface traffic capture gaps

**Tools Used**

Ubuntu 24.04, Suricata 8.0.3, Docker \& Docker Compose, ELK Stack 8.15.0 
(Elasticsearch, Logstash, Kibana), Filebeat 8.15.0, VirtualBox

**Setup Overview**

1\. Suricata installed on an Ubuntu VM, configured to monitor both the primary &#x20;  network interface and loopback

2\. Custom rule (`suricata/local.rules`) added to detect port scans via SYN &#x20;  packet thresholds

3\. ELK stack deployed via Docker Compose on a separate host machine

4\. Filebeat configured to ship `eve.json` alerts across the network to Logstash

5\. Kibana dashboard built to visualize alerts by signature type and over time

**Sample Detections**

See `suricata/sample-alerts/` for real captured alert JSON:

\- `http-alert-testmyids.json` - Emerging Threats signature match on HTTP response content

\- `portscan-alert-custom-rule.json` - Custom rule detecting nmap SYN scan behavior

**Screenshots**

See `screenshots/` for the full walkthrough, including the Kibana dashboard.

**Key Troubleshooting Highlights**

\- \*\*YAML indentation bug\*\*: A stray space broke Suricata's config parser - fixed &#x20; by diffing against a backup

\- \*\*Rule file path mismatch\*\*: Custom rule wasn't loading because it was placed &#x20; in the wrong directory (`/etc/suricata/rules/` vs `/var/lib/suricata/rules/`)

\- \*\*Loopback traffic blindness\*\*: Self-scanning the VM's own IP routed traffic &#x20; through the `lo` interface, which Suricata wasn't monitoring - fixed by adding &#x20; a second af-packet listener

**Full Write-up**

Read the detailed article on Medium: \[link here once published]

