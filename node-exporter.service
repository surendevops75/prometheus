# Node Exporter Systemd Service Example

This file creates a **systemd service** for Node Exporter on Linux.

Node Exporter is a Prometheus exporter that collects operating system and hardware metrics from Linux servers.

It exposes metrics such as:

- CPU Usage
- Memory Usage
- Disk Usage
- Filesystem Statistics
- Network Statistics
- Load Average
- System Uptime

Prometheus scrapes these metrics and stores them for monitoring and alerting.

Using a systemd service allows Node Exporter to:

- Start automatically after server reboot
- Restart automatically if it crashes
- Run in the background
- Be managed using Linux service commands

This is the standard production deployment method.

---

# Systemd Service File

```ini
# --------------------------------------------------------
# UNIT SECTION
# --------------------------------------------------------

[Unit]

# Service description
Description=A simple custom service

# Start service after networking is available
After=network.target

# --------------------------------------------------------
# SERVICE SECTION
# --------------------------------------------------------

[Service]

# Simple long-running process
Type=simple

# Node Exporter binary
ExecStart=/opt/node_exporter/node_exporter

# Automatically restart if service crashes
Restart=always

# --------------------------------------------------------
# INSTALL SECTION
# --------------------------------------------------------

[Install]

# Start service during normal system boot
WantedBy=multi-user.target
```

---

# What Is Node Exporter?

Node Exporter is an exporter for:

:contentReference[oaicite:0]{index=0}

It collects Linux system metrics and exposes them through:

```text
http://SERVER-IP:9100/metrics
```

Prometheus then scrapes these metrics.

---

# Monitoring Architecture

```text
Linux Server
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
Grafana
(port 3000)
```

---

# Unit Section

```ini
[Unit]
```

Contains service metadata and dependencies.

---

## Description

```ini
Description=A simple custom service
```

Human-readable service description.

Visible using:

```bash
systemctl status node_exporter
```

Example:

```text
Loaded: loaded
Active: active (running)

Description: A simple custom service
```

---

## After

```ini
After=network.target
```

Ensures networking starts before Node Exporter.

Startup sequence:

```text
Server Boot
     ↓
Network Starts
     ↓
Node Exporter Starts
```

This ensures metrics endpoint becomes reachable immediately.

---

# Service Section

```ini
[Service]
```

Defines how Node Exporter runs.

---

## Type

```ini
Type=simple
```

Means:

```text
Start Process
      ↓
Keep Running
```

Suitable for:

- Node Exporter
- Prometheus
- Grafana
- Jenkins Agents

---

## ExecStart

```ini
ExecStart=/opt/node_exporter/node_exporter
```

Command executed when service starts.

Breakdown:

### Binary Location

```text
/opt/node_exporter/node_exporter
```

This is the Node Exporter executable.

When systemd starts the service:

```text
systemctl start node_exporter
```

it runs:

```text
/opt/node_exporter/node_exporter
```

which starts listening on:

```text
Port 9100
```

---

## Restart

```ini
Restart=always
```

Automatically restarts Node Exporter if:

- Process crashes
- Server issues occur
- Unexpected exit happens

Flow:

```text
Node Exporter Stops
         ↓
systemd Detects Failure
         ↓
Automatically Restarts
         ↓
Node Exporter Running Again
```

Benefits:

- Higher availability
- Automatic recovery
- Reduced monitoring downtime

---

# Install Section

```ini
[Install]
```

Defines when the service starts.

---

## WantedBy

```ini
WantedBy=multi-user.target
```

Means:

```text
Start during normal Linux boot
```

Boot sequence:

```text
Linux Boot
      ↓
multi-user.target
      ↓
Node Exporter Starts
```

Equivalent to traditional:

```text
Runlevel 3
```

---

# Service Lifecycle Commands

## Start Service

```bash
sudo systemctl start node_exporter
```

---

## Stop Service

```bash
sudo systemctl stop node_exporter
```

---

## Restart Service

```bash
sudo systemctl restart node_exporter
```

---

## Check Status

```bash
sudo systemctl status node_exporter
```

Expected output:

```text
● node_exporter.service
   Loaded: loaded
   Active: active (running)
```

---

## Enable Auto Start

```bash
sudo systemctl enable node_exporter
```

Starts service automatically after reboot.

---

## Disable Auto Start

```bash
sudo systemctl disable node_exporter
```

Stops automatic startup.

---

# View Logs

Display logs:

```bash
sudo journalctl -u node_exporter
```

Live logs:

```bash
sudo journalctl -u node_exporter -f
```

Useful for:

- Debugging
- Startup failures
- Port conflicts

---

# Verify Node Exporter

Check service:

```bash
systemctl status node_exporter
```

Check listening port:

```bash
netstat -lntp | grep 9100
```

or

```bash
ss -lntp | grep 9100
```

---

# Test Metrics Endpoint

Open:

```text
http://SERVER-IP:9100/metrics
```

Expected output:

```text
node_cpu_seconds_total
node_memory_MemAvailable_bytes
node_filesystem_size_bytes
```

Thousands of metrics will be displayed.

---

# Prometheus Integration

Prometheus configuration:

```yaml
scrape_configs:
  - job_name: node-exporter

    static_configs:
      - targets:
        - SERVER-IP:9100
```

Prometheus workflow:

```text
Node Exporter
      ↓
Prometheus Scrapes Metrics
      ↓
Stores Metrics
      ↓
Grafana Visualizes
```

---

# Common Metrics Collected

## CPU Metrics

```text
node_cpu_seconds_total
```

Tracks CPU utilization.

---

## Memory Metrics

```text
node_memory_MemAvailable_bytes
```

Tracks available memory.

---

## Disk Metrics

```text
node_filesystem_avail_bytes
```

Tracks free disk space.

---

## Network Metrics

```text
node_network_receive_bytes_total
```

Tracks incoming network traffic.

---

## System Uptime

```text
node_time_seconds
```

Tracks server uptime.

---

# Real DevOps Use Cases

## EC2 Monitoring

Monitor:

- CPU
- Memory
- Disk
- Network

for AWS EC2 instances.

---

## Kubernetes Worker Monitoring

Install Node Exporter on worker nodes.

Monitor cluster infrastructure.

---

## Production Alerting

Generate alerts for:

- High CPU usage
- High memory usage
- Disk full
- Node unreachable

---

# Best Practices

✅ Run Node Exporter as systemd service

✅ Enable automatic restart

✅ Enable startup on boot

✅ Restrict access using Security Groups

✅ Monitor service health

✅ Integrate with Prometheus

---

# Benefits of Using systemd

- Automatic startup
- Automatic recovery
- Centralized service management
- Log management through journalctl
- Production-grade reliability

---

# How to Deploy

Create service file:

```bash
sudo vi /etc/systemd/system/node_exporter.service
```

Paste the configuration.

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable service:

```bash
sudo systemctl enable node_exporter
```

Start service:

```bash
sudo systemctl start node_exporter
```

Verify:

```bash
sudo systemctl status node_exporter
```

Test metrics endpoint:

```text
http://SERVER-IP:9100/metrics
```

---

# Why This Service File Is Important

This systemd configuration demonstrates how to run:

- :contentReference[oaicite:1]{index=1}
- :contentReference[oaicite:2]{index=2}

in a production Linux environment.

These concepts are foundational for:

- DevOps Engineering
- Site Reliability Engineering (SRE)
- Infrastructure Monitoring
- Linux Administration
- Cloud Operations