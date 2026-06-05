# Prometheus Configuration Example

This configuration file defines how Prometheus collects metrics, discovers targets, loads alert rules, and integrates with Alertmanager.

Prometheus is an open-source monitoring and alerting system widely used in DevOps, Kubernetes, and Cloud environments.

This configuration demonstrates:

- Global Prometheus settings
- Alertmanager integration
- Alert rule loading
- Static target monitoring
- AWS EC2 Service Discovery
- Dynamic target labeling

---

# Prometheus Configuration

```yaml
# --------------------------------------------------------
# GLOBAL SETTINGS
# --------------------------------------------------------

global:

  # Collect metrics every 15 seconds
  scrape_interval: 15s

  # Evaluate alert rules every 15 seconds
  evaluation_interval: 15s

# --------------------------------------------------------
# ALERTMANAGER CONFIGURATION
# --------------------------------------------------------

alerting:

  alertmanagers:

    - static_configs:

        - targets:

          # Alertmanager endpoint
          - "localhost:9093"

# --------------------------------------------------------
# ALERT RULE FILES
# --------------------------------------------------------

rule_files:

  # Load all alert rules
  - "alert-rules/*.yaml"

# --------------------------------------------------------
# SCRAPE CONFIGURATIONS
# --------------------------------------------------------

scrape_configs:

  # --------------------------------------------------------
  # PROMETHEUS SELF MONITORING
  # --------------------------------------------------------

  - job_name: "prometheus"

    static_configs:

      - targets:
          - "localhost:9090"

        labels:
          app: "prometheus"

  # --------------------------------------------------------
  # AWS EC2 SERVICE DISCOVERY
  # --------------------------------------------------------

  - job_name: ec2_sd

    ec2_sd_configs:

      - region: us-east-1

        port: 9100

        filters:

          - name: "tag:Monitoring"

            values:
              - "true"

    relabel_configs:

      - source_labels:
          - __meta_ec2_instance_id

        target_label: instance_id

      - source_labels:
          - __meta_ec2_tag_Name

        target_label: name
```

---

# Architecture Overview

```text
EC2 Instances
      │
      ▼
Node Exporter
(port 9100)
      │
      ▼
Prometheus
(port 9090)
      │
      ▼
Alert Rules
      │
      ▼
Alertmanager
(port 9093)
      │
      ▼
Email / Slack / Teams
```

---

# Global Configuration

```yaml
global:
```

Defines default settings used across Prometheus.

---

## scrape_interval

```yaml
scrape_interval: 15s
```

Controls how frequently Prometheus collects metrics.

Current setting:

```text
Every 15 seconds
```

Example:

```text
00:00:00
00:00:15
00:00:30
00:00:45
```

Prometheus pulls metrics from monitored targets at these intervals.

---

## evaluation_interval

```yaml
evaluation_interval: 15s
```

Determines how often Prometheus evaluates alert rules.

Current setting:

```text
Every 15 seconds
```

Used for:

- CPU alerts
- Memory alerts
- Disk alerts
- Application alerts

---

# Alertmanager Configuration

```yaml
alerting:
```

Defines where Prometheus sends alerts.

---

## Alertmanager Target

```yaml
localhost:9093
```

Prometheus sends alerts to Alertmanager.

Flow:

```text
Prometheus
      ↓
Alertmanager
      ↓
Email
Slack
Teams
PagerDuty
```

---

# Rule Files

```yaml
rule_files:
```

Loads alert rule definitions.

Current configuration:

```yaml
alert-rules/*.yaml
```

Meaning:

```text
Load all YAML files
inside alert-rules directory
```

Example:

```text
alert-rules/
├── cpu.yaml
├── memory.yaml
├── disk.yaml
└── node.yaml
```

---

# Scrape Configurations

```yaml
scrape_configs:
```

Defines what Prometheus should monitor.

Each:

```yaml
job_name:
```

represents a monitoring target group.

---

# Prometheus Self Monitoring

```yaml
job_name: prometheus
```

Prometheus monitors itself.

Target:

```yaml
localhost:9090
```

Prometheus exposes its own metrics on:

```text
http://localhost:9090/metrics
```

Useful for monitoring:

- Scrape duration
- Memory usage
- Query performance
- Storage usage

---

# Labels

```yaml
labels:
```

Adds metadata to metrics.

Example:

```yaml
app: prometheus
```

Generated metric:

```text
up{app="prometheus"}
```

Labels help with:

- Filtering
- Dashboards
- Alerting

---

# AWS EC2 Service Discovery

```yaml
job_name: ec2_sd
```

Uses AWS Service Discovery.

Instead of manually adding servers:

```yaml
targets:
  - server1:9100
  - server2:9100
```

Prometheus discovers instances automatically.

---

# ec2_sd_configs

```yaml
ec2_sd_configs:
```

Enables AWS EC2 discovery.

Prometheus calls AWS APIs and retrieves EC2 instance details.

---

## Region

```yaml
region: us-east-1
```

AWS region where instances are located.

Prometheus searches:

```text
US East (N. Virginia)
```

for EC2 instances.

---

## Port

```yaml
port: 9100
```

Prometheus expects:

```text
Node Exporter
```

to be running on:

```text
Port 9100
```

Example:

```text
172.31.10.15:9100
```

---

# EC2 Filtering

```yaml
filters:
```

Filters discovered EC2 instances.

Current filter:

```yaml
tag:Monitoring = true
```

Only instances with:

```text
Monitoring=true
```

tag are monitored.

Example:

| Instance | Monitoring Tag | Scraped |
|-----------|--------------|----------|
| Web Server | true | ✅ |
| App Server | true | ✅ |
| Database | false | ❌ |

---

# Relabel Configurations

```yaml
relabel_configs:
```

Transforms AWS metadata into Prometheus labels.

---

## Instance ID Label

```yaml
__meta_ec2_instance_id
```

Creates:

```yaml
instance_id
```

label.

Example:

```text
instance_id=i-0abc123456
```

Useful for:

- Alerts
- Dashboards
- Troubleshooting

---

## Name Tag Label

```yaml
__meta_ec2_tag_Name
```

Creates:

```yaml
name
```

label.

Example:

```text
name=web-server
```

Instead of seeing:

```text
i-0abc123456
```

you see:

```text
web-server
```

which is easier to understand.

---

# Monitoring Workflow

```text
EC2 Instance
      │
      ▼
Node Exporter
(port 9100)
      │
      ▼
Prometheus Discovery
      │
      ▼
Metric Collection
      │
      ▼
Rule Evaluation
      │
      ▼
Alertmanager
      │
      ▼
Notifications
```

---

# Real DevOps Use Cases

## Infrastructure Monitoring

Monitor:

- CPU
- Memory
- Disk
- Network

for EC2 instances.

---

## Auto Scaling Environments

New EC2 instances:

```text
Launch
   ↓
Receive Monitoring=true tag
   ↓
Automatically discovered
   ↓
Automatically monitored
```

No manual configuration required.

---

## Kubernetes Worker Monitoring

Monitor EKS worker nodes using:

```text
Node Exporter
```

and EC2 discovery.

---

## Alerting

Examples:

- CPU > 80%
- Memory > 90%
- Disk Usage > 85%
- Node Down

---

# Best Practices

✅ Use Service Discovery

✅ Use Tags for Monitoring Selection

✅ Add Meaningful Labels

✅ Store Alert Rules Separately

✅ Integrate Alertmanager

✅ Monitor Prometheus Itself

---

# Benefits of This Configuration

- Dynamic EC2 discovery
- Automated monitoring
- Centralized alerting
- Easy scaling
- Reduced manual effort
- Production-ready monitoring setup

---

# How to Validate Configuration

Check configuration:

```bash
promtool check config prometheus.yml
```

Reload Prometheus:

```bash
curl -X POST http://localhost:9090/-/reload
```

Verify targets:

```text
http://localhost:9090/targets
```

Verify discovered instances:

```text
Status → Targets
```

inside Prometheus UI.

---

# Why This Configuration Is Important

This configuration demonstrates production-grade monitoring concepts using:

- :contentReference[oaicite:0]{index=0}
- :contentReference[oaicite:1]{index=1}
- :contentReference[oaicite:2]{index=2}

These concepts are foundational for:

- DevOps Engineering
- Site Reliability Engineering (SRE)
- Kubernetes Monitoring
- Cloud Infrastructure Monitoring
- Production Operations