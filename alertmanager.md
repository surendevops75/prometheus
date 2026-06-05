# Alertmanager Gmail Email Configuration Example

This configuration file enables Alertmanager to send email notifications through Gmail whenever Prometheus alerts are triggered.

Alertmanager is responsible for:

- Receiving alerts from Prometheus
- Grouping alerts
- Routing alerts
- Sending notifications
- Managing alert silencing

In this example:

```text
Prometheus
      ↓
Alertmanager
      ↓
Gmail SMTP Server
      ↓
info@joindevops.com
```

Whenever an alert fires, Alertmanager sends an email notification.

---

# Important Security Note

⚠️ Never commit real passwords or Gmail App Passwords to Git repositories.

Instead use:

- Kubernetes Secrets
- Environment Variables
- Vault
- AWS Secrets Manager

Your current configuration contains a real Gmail App Password and should be rotated immediately if this file was pushed to GitHub.

---

# Alertmanager Configuration

```yaml
# --------------------------------------------------------
# GLOBAL SETTINGS
# --------------------------------------------------------

global:

  # Gmail SMTP server
  smtp_smarthost: smtp.gmail.com:587

  # Sender email
  smtp_from: m.sivakmrreddy@gmail.com

  # Gmail username
  smtp_auth_username: m.sivakmrreddy@gmail.com

  # Gmail App Password
  smtp_auth_password: <APP_PASSWORD>

  # SMTP identity
  smtp_auth_identity: m.sivakmrreddy@gmail.com

  # Enable TLS
  smtp_require_tls: true

  # Alert resolution timeout
  resolve_timeout: 5m

# --------------------------------------------------------
# ALERT ROUTING
# --------------------------------------------------------

route:

  # Default receiver
  receiver: 'gmail-alerts'

# --------------------------------------------------------
# RECEIVERS
# --------------------------------------------------------

receivers:

- name: 'gmail-alerts'

  email_configs:

  - to: info@joindevops.com

    # Send email when alert resolves
    send_resolved: true

    headers:

      Subject: '[{{ .Status | toUpper }}] {{ .CommonLabels.alertname }} ({{ .CommonLabels.severity }})'
```

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
Gmail SMTP
      ↓
Email Notification
```

---

# Global Configuration

```yaml
global:
```

Contains SMTP settings used by Alertmanager.

These settings apply to all email receivers.

---

# smtp_smarthost

```yaml
smtp_smarthost: smtp.gmail.com:587
```

Defines the SMTP server used for email delivery.

Breakdown:

```text
smtp.gmail.com
```

Gmail SMTP server.

Port:

```text
587
```

Used for:

```text
STARTTLS
```

secure email transmission.

---

# smtp_from

```yaml
smtp_from: m.sivakmrreddy@gmail.com
```

Email sender address.

Recipients will see:

```text
From:
m.sivakmrreddy@gmail.com
```

---

# smtp_auth_username

```yaml
smtp_auth_username: m.sivakmrreddy@gmail.com
```

Gmail account used for authentication.

---

# smtp_auth_password

```yaml
smtp_auth_password: <APP_PASSWORD>
```

Used to authenticate with Gmail.

Important:

```text
Use Gmail App Password
NOT Gmail Login Password
```

Steps:

```text
Google Account
      ↓
Security
      ↓
2-Factor Authentication
      ↓
App Passwords
      ↓
Generate Password
```

---

# smtp_auth_identity

```yaml
smtp_auth_identity: m.sivakmrreddy@gmail.com
```

Identity presented to SMTP server.

Typically same as username.

---

# smtp_require_tls

```yaml
smtp_require_tls: true
```

Enforces encrypted communication.

Flow:

```text
Alertmanager
      ↓
TLS Encryption
      ↓
Gmail SMTP
```

Without TLS:

```text
Credentials travel insecurely
```

---

# resolve_timeout

```yaml
resolve_timeout: 5m
```

Determines how long Alertmanager waits before considering an alert resolved.

Current setting:

```text
5 minutes
```

---

# Route Configuration

```yaml
route:
```

Controls where alerts are sent.

---

# Receiver

```yaml
receiver: gmail-alerts
```

Default destination for alerts.

Flow:

```text
Prometheus Alert
        ↓
Route
        ↓
gmail-alerts Receiver
```

---

# Receivers

```yaml
receivers:
```

Receivers define notification destinations.

Examples:

- Email
- Slack
- Teams
- PagerDuty
- Webhook

This configuration uses:

```text
Email Receiver
```

---

# Receiver Name

```yaml
name: gmail-alerts
```

Unique receiver identifier.

Referenced by:

```yaml
route:
  receiver: gmail-alerts
```

---

# Email Configuration

```yaml
email_configs:
```

Defines email notification settings.

---

# Destination Email

```yaml
to: info@joindevops.com
```

Recipient address.

All alerts will be sent here.

---

# send_resolved

```yaml
send_resolved: true
```

Behavior:

When alert fires:

```text
CPU High
      ↓
Email Sent
```

When alert recovers:

```text
CPU Normal
      ↓
Resolved Email Sent
```

Benefits:

- Know when issue starts
- Know when issue ends

---

# Custom Subject Line

```yaml
Subject:
```

Customizes email subject.

Current template:

```yaml
'[{{ .Status | toUpper }}] {{ .CommonLabels.alertname }} ({{ .CommonLabels.severity }})'
```

---

# Example Email Subjects

## Alert Firing

```text
[FIRING] HighCPUUsage (critical)
```

---

## Alert Resolved

```text
[RESOLVED] HighCPUUsage (critical)
```

---

# Template Variables

## Status

```yaml
{{ .Status }}
```

Values:

```text
firing
resolved
```

Converted to uppercase:

```text
FIRING
RESOLVED
```

---

## Alert Name

```yaml
{{ .CommonLabels.alertname }}
```

Example:

```text
HighCPUUsage
DiskSpaceLow
NodeDown
```

---

## Severity

```yaml
{{ .CommonLabels.severity }}
```

Example:

```text
warning
critical
info
```

---

# Alert Flow

```text
Node Exporter
      ↓
Prometheus
      ↓
Alert Rule Triggered
      ↓
Alertmanager
      ↓
Gmail SMTP
      ↓
Email Delivered
```

---

# Example Alert Email

Subject:

```text
[FIRING] HighCPUUsage (critical)
```

Body:

```text
Alert Name: HighCPUUsage

Instance: web-server

Severity: critical

Description:
CPU usage exceeded 80%
```

---

# Real DevOps Use Cases

## Infrastructure Monitoring

Alerts for:

- CPU Usage
- Memory Usage
- Disk Usage
- Network Issues

---

## Server Availability

Alerts when:

```text
Node Down
Instance Unreachable
Exporter Down
```

---

## Kubernetes Monitoring

Alerts for:

```text
Pod CrashLoopBackOff
Node Not Ready
High Memory Usage
```

---

## Production Incident Notification

Immediate notification to:

- DevOps Team
- SRE Team
- Platform Team

---

# Best Practices

✅ Use Gmail App Password

✅ Enable TLS

✅ Enable send_resolved

✅ Use Custom Subject Lines

✅ Store Credentials in Secrets

✅ Rotate SMTP Credentials Regularly

❌ Never Commit Passwords to Git

---

# How to Validate Configuration

Check Alertmanager configuration:

```bash
amtool check-config alertmanager.yml
```

Restart Alertmanager:

```bash
sudo systemctl restart alertmanager
```

Verify service:

```bash
sudo systemctl status alertmanager
```

Check logs:

```bash
journalctl -u alertmanager -f
```

---

# Why This Configuration Is Important

This configuration demonstrates production alerting concepts using:

- :contentReference[oaicite:0]{index=0}
- :contentReference[oaicite:1]{index=1}
- Gmail SMTP integration

These concepts are fundamental for:

- DevOps Engineering
- Site Reliability Engineering (SRE)
- Production Monitoring
- Incident Management
- Infrastructure Operations