# Prometheus Systemd Service Example

This file creates a **systemd service** for Prometheus on Linux.

Systemd is the default service manager used in most modern Linux distributions such as:

- Ubuntu
- Amazon Linux
- RHEL
- CentOS
- Rocky Linux

Using a systemd service allows Prometheus to:

- Start automatically during server boot
- Restart automatically if it crashes
- Run in the background as a managed service
- Be controlled using standard Linux commands

This is the recommended production approach for running Prometheus on Linux servers.

---

# Systemd Service File

```ini
# --------------------------------------------------------
# UNIT SECTION
# --------------------------------------------------------

[Unit]

# Service description
Description=A simple custom service

# Start service only after networking is available
After=network.target

# --------------------------------------------------------
# SERVICE SECTION
# --------------------------------------------------------

[Service]

# Service type
Type=simple

# Command used to start Prometheus
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml

# Automatically restart service if it stops
Restart=always

# --------------------------------------------------------
# INSTALL SECTION
# --------------------------------------------------------

[Install]

# Start service automatically during system boot
WantedBy=multi-user.target
```

---

# What Is systemd?

systemd is the Linux service manager responsible for:

- Starting services
- Stopping services
- Monitoring services
- Restarting failed services
- Managing boot processes

Examples of systemd-managed services:

```text
nginx
docker
jenkins
prometheus
grafana
node_exporter
```

---

# Unit Section

```ini
[Unit]
```

Contains metadata and dependency information.

Defines:

- Service description
- Startup order
- Service relationships

---

## Description

```ini
Description=A simple custom service
```

Human-readable service description.

Visible using:

```bash
systemctl status prometheus
```

Example output:

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

Ensures networking is available before Prometheus starts.

Startup sequence:

```text
Server Boot
     ↓
Network Starts
     ↓
Prometheus Starts
```

Why?

Prometheus needs network access to:

- Scrape targets
- Connect to Alertmanager
- Access exporters

---

# Service Section

```ini
[Service]
```

Defines how the service runs.

---

## Type

```ini
Type=simple
```

Indicates:

```text
Start process
      ↓
Keep process running
```

Suitable for:

- Prometheus
- Grafana
- Node Exporter
- Custom applications

---

## ExecStart

```ini
ExecStart=/opt/prometheus/prometheus --config.file=/opt/prometheus/prometheus.yml
```

Command executed when service starts.

Breakdown:

### Prometheus Binary

```text
/opt/prometheus/prometheus
```

Prometheus executable.

---

### Configuration File

```text
/opt/prometheus/prometheus.yml
```

Main Prometheus configuration file.

Contains:

- Scrape jobs
- Alert rules
- Service discovery
- Alertmanager settings

---

### Startup Flow

```text
systemctl start prometheus
             ↓
ExecStart executed
             ↓
Prometheus process starts
             ↓
Metrics collection begins
```

---

## Restart

```ini
Restart=always
```

Automatically restarts Prometheus if:

- Process crashes
- Unexpected exit occurs
- Application failure happens

Flow:

```text
Prometheus Crashes
         ↓
systemd Detects Failure
         ↓
Automatically Restarts
         ↓
Prometheus Running Again
```

Benefits:

- High availability
- Reduced downtime
- Automatic recovery

---

# Install Section

```ini
[Install]
```

Defines when service should start.

---

## WantedBy

```ini
WantedBy=multi-user.target
```

Means:

```text
Start during normal Linux boot
```

Equivalent to:

```text
Runlevel 3
```

Startup flow:

```text
Server Boot
      ↓
multi-user.target reached
      ↓
Prometheus service starts
```

---

# Service Lifecycle

## Start Service

```bash
sudo systemctl start prometheus
```

---

## Stop Service

```bash
sudo systemctl stop prometheus
```

---

## Restart Service

```bash
sudo systemctl restart prometheus
```

---

## Reload Service

```bash
sudo systemctl reload prometheus
```

---

## Check Status

```bash
sudo systemctl status prometheus
```

Example:

```text
● prometheus.service
   Loaded: loaded
   Active: active (running)
```

---

## Enable Auto Start

```bash
sudo systemctl enable prometheus
```

Starts Prometheus automatically after every reboot.

---

## Disable Auto Start

```bash
sudo systemctl disable prometheus
```

Prevents automatic startup during boot.

---

# View Logs

Using journalctl:

```bash
sudo journalctl -u prometheus
```

Live logs:

```bash
sudo journalctl -u prometheus -f
```

Useful for:

- Troubleshooting
- Startup issues
- Configuration errors

---

# Service Boot Flow

```text
Linux Server Boot
        ↓
network.target reached
        ↓
Prometheus Service Starts
        ↓
Prometheus Loads Configuration
        ↓
Targets Discovered
        ↓
Metrics Collection Starts
```

---

# Real DevOps Use Cases

## Monitoring Servers

Run Prometheus as a managed Linux service.

---

## Production Monitoring

Automatically restart Prometheus after failures.

---

## EC2 Monitoring

Deploy Prometheus on:

- Amazon Linux
- Ubuntu
- RHEL

and manage it through systemd.

---

## Infrastructure Monitoring

Used with:

- Node Exporter
- Alertmanager
- Grafana

to build a complete monitoring stack.

---

# Best Practices

✅ Enable automatic restart

✅ Start after networking is available

✅ Run Prometheus through systemd

✅ Monitor service health

✅ Store logs using journalctl

✅ Enable service at boot

---

# Benefits of Using systemd

- Automatic startup
- Automatic restart
- Centralized logging
- Service management
- Production-grade reliability

---

# How to Deploy

Create service file:

```bash
sudo vi /etc/systemd/system/prometheus.service
```

Paste the configuration.

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable service:

```bash
sudo systemctl enable prometheus
```

Start service:

```bash
sudo systemctl start prometheus
```

Verify:

```bash
sudo systemctl status prometheus
```

Access Prometheus:

```text
http://SERVER-IP:9090
```

---

# Why This Service File Is Important

This systemd configuration demonstrates how to run:

- :contentReference[oaicite:0]{index=0}

as a production-grade Linux service.

These concepts are fundamental for:

- DevOps Engineering
- Site Reliability Engineering (SRE)
- Infrastructure Monitoring
- Production Linux Administration