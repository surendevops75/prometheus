# Alertmanager Systemd Service Example

This file creates a **systemd service** for Alertmanager on Linux.

Alertmanager is a component of the Prometheus ecosystem responsible for:

- Receiving alerts from Prometheus
- Grouping alerts
- Deduplicating alerts
- Routing alerts
- Sending notifications
- Managing alert silencing

Using a systemd service allows Alertmanager to:

- Start automatically during server boot
- Restart automatically if it crashes
- Run continuously in the background
- Be managed using standard Linux service commands

This is the recommended production deployment method.

---

# Systemd Service File

```ini
# --------------------------------------------------------
# UNIT SECTION
# --------------------------------------------------------

[Unit]

# Service description
Description=A simple custom service

# Start service after network becomes available
After=network.target

# --------------------------------------------------------
# SERVICE SECTION
# --------------------------------------------------------

[Service]

# Long-running foreground process
Type=simple

# Alertmanager startup command
ExecStart=/opt/alertmanager/alertmanager --config.file=/opt/alertmanager/alertmanager.yml

# Automatically restart if process exits
Restart=always

# --------------------------------------------------------
# INSTALL SECTION
# --------------------------------------------------------

[Install]

# Start automatically during normal system boot
WantedBy=multi-user.target
```

---

# What Is Alertmanager?

Alertmanager is part of the:

:contentReference[oaicite:0]{index=0}

monitoring ecosystem.

It receives alerts from Prometheus and sends notifications through:

- Email
- Slack
- Microsoft Teams
- PagerDuty
- Webhooks
- OpsGenie

---

# Monitoring Architecture

```text
Node Exporter
      ↓
Prometheus
      ↓
Alert Rules
      ↓
Alertmanager
      ↓
Email / Slack / Teams
```

---

# Unit Section

```ini
[Unit]
```

Contains service metadata and startup dependencies.

---

## Description

```ini
Description=A simple custom service
```

Human-readable description of the service.

Visible using:

```bash
systemctl status alertmanager
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

Ensures networking is available before Alertmanager starts.

Boot sequence:

```text
Linux Server Boot
         ↓
Network Starts
         ↓
Alertmanager Starts
```

Why?

Alertmanager must communicate with:

- Prometheus
- SMTP Servers
- Slack APIs
- Webhooks

---

# Service Section

```ini
[Service]
```

Defines how Alertmanager runs.

---

## Type

```ini
Type=simple
```

Indicates:

```text
Start Process
      ↓
Keep Running
```

Suitable for:

- Prometheus
- Alertmanager
- Grafana
- Node Exporter

---

## ExecStart

```ini
ExecStart=/opt/alertmanager/alertmanager --config.file=/opt/alertmanager/alertmanager.yml
```

Command executed when service starts.

---

### Alertmanager Binary

```text
/opt/alertmanager/alertmanager
```

Main Alertmanager executable.

---

### Configuration File

```text
/opt/alertmanager/alertmanager.yml
```

Contains:

- Receivers
- Routes
- Email settings
- Slack settings
- Notification policies

---

### Startup Flow

```text
systemctl start alertmanager
              ↓
ExecStart executes
              ↓
Alertmanager loads configuration
              ↓
Alertmanager starts listening
```

Default UI:

```text
http://SERVER-IP:9093
```

---

## Restart

```ini
Restart=always
```

Automatically restarts Alertmanager if:

- Process crashes
- Server issues occur
- Unexpected exit happens

Recovery flow:

```text
Alertmanager Stops
         ↓
systemd Detects Failure
         ↓
Automatically Restarts
         ↓
Alertmanager Running Again
```

Benefits:

- High availability
- Automatic recovery
- Reduced downtime

---

# Install Section

```ini
[Install]
```

Defines startup behavior.

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
Alertmanager Starts
```

Equivalent to:

```text
Traditional Runlevel 3
```

---

# Service Lifecycle Commands

## Start Service

```bash
sudo systemctl start alertmanager
```

---

## Stop Service

```bash
sudo systemctl stop alertmanager
```

---

## Restart Service

```bash
sudo systemctl restart alertmanager
```

---

## Check Status

```bash
sudo systemctl status alertmanager
```

Expected output:

```text
● alertmanager.service
   Loaded: loaded
   Active: active (running)
```

---

## Enable Auto Start

```bash
sudo systemctl enable alertmanager
```

Starts service automatically after reboot.

---

## Disable Auto Start

```bash
sudo systemctl disable alertmanager
```

Disables startup during boot.

---

# View Logs

Display logs:

```bash
sudo journalctl -u alertmanager
```

Follow logs in real time:

```bash
sudo journalctl -u alertmanager -f
```

Useful for:

- SMTP errors
- Configuration issues
- Notification failures

---

# Verify Alertmanager

Check service:

```bash
systemctl status alertmanager
```

Check listening port:

```bash
ss -lntp | grep 9093
```

Expected:

```text
*:9093
```

---

# Access Alertmanager UI

Open browser:

```text
http://SERVER-IP:9093
```

You can view:

- Active Alerts
- Silenced Alerts
- Notification Status

---

# Prometheus Integration

Prometheus configuration:

```yaml
alerting:
  alertmanagers:
    - static_configs:
        - targets:
          - localhost:9093
```

Alert flow:

```text
Prometheus
      ↓
Alertmanager
      ↓
Email / Slack
```

---

# Real DevOps Use Cases

## Infrastructure Monitoring

Alert on:

- High CPU
- High Memory
- Disk Full
- Node Down

---

## Kubernetes Monitoring

Alert on:

- Pod Failures
- Node Not Ready
- Deployment Issues
- Resource Exhaustion

---

## Incident Notification

Notify:

- DevOps Team
- SRE Team
- Platform Engineers

through:

- Email
- Slack
- PagerDuty

---

# Best Practices

✅ Run Alertmanager as systemd service

✅ Enable automatic restart

✅ Enable startup at boot

✅ Validate configuration before restart

✅ Monitor Alertmanager health

✅ Store credentials securely

---

# Validate Configuration

Before restarting:

```bash
amtool check-config /opt/alertmanager/alertmanager.yml
```

Expected:

```text
Checking configuration
SUCCESS
```

---

# Deployment Steps

Create service file:

```bash
sudo vi /etc/systemd/system/alertmanager.service
```

Paste configuration.

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Enable service:

```bash
sudo systemctl enable alertmanager
```

Start service:

```bash
sudo systemctl start alertmanager
```

Verify:

```bash
sudo systemctl status alertmanager
```

Access UI:

```text
http://SERVER-IP:9093
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
- Incident Management
- Production Operations