# SIEM Environment Setup (Windows & Linux VMs with Splunk)

This repository documents the setup process for a lab environment built to simulate real-world systems monitored by a SIEM (Security Information and Event Management) platform — Splunk. The environment includes Windows 10 and Linux virtual machines.

---

## 📁 Directory Structure

- `/logs/` — Collected log files from systems and applications
- `/screenshots/` — Visual proof of configuration steps and verification
- `/reports/` — Reports generated from Splunk or manual analysis
- `/hints/` — Tips and configuration tricks that helped during the setup

---

## 🖥️ Environment Overview

- **Host Machine**: [Your base OS, e.g., Windows 11 or Ubuntu 22.04]
- **Hypervisor**: [e.g., VMware Workstation / VirtualBox]
- **Guest VMs**:
  - **Windows 10 home**
  - **Ubuntu Server 20.04**
- **SIEM Tool**: Splunk Enterprise (Free Tier for lab use)

---

## 🛠️ VM Setup Steps

### 1. Windows 10 VM

- Installed Windows 10 home on VM.
- Enabled RDP and ensured network connectivity.
- Set static IP for easier Splunk data forwarding.
- Installed Sysmon for enhanced logging:
  - Downloaded from Microsoft Sysinternals.
  - Configured with custom config (`sysmon-config.xml`) to capture detailed events.
- Installed Splunk Universal Forwarder:
  - Configured to forward logs to Splunk server on Linux VM.
  - Verified forwarding of Application, Security, and System logs.

### 2. Linux VM (Ubuntu Server)

- Installed Ubuntu Server 20.04 on VM.
- Assigned static IP and set up SSH access.
- Installed `auditd` and `rsyslog` for logging system events.
- Configured Splunk Universal Forwarder:
  - Forwarded `/var/log/auth.log`, `/var/log/syslog`, and `/var/log/apache2/` (if web server installed).
- Created test users and ran login attempts to generate logs.

---

## 🔍 Splunk SIEM Setup (on Linux VM)

- Downloaded and installed **Splunk Enterprise**:
  - From [https://www.splunk.com/](https://www.splunk.com/)
  - Installed via `.deb` package
  - Enabled boot start and web UI on port 8000
- Configured Splunk to:
  - Receive data via TCP/9997 (from forwarders)
  - Parse Windows Event Logs using the Splunk Add-on for Windows
  - Parse Linux logs with custom source types
- Created basic dashboards for:
  - Login activity
  - Suspicious PowerShell usage
  - Failed SSH attempts

---

## ✅ Verification

- Verified logs from both VMs were arriving in Splunk.
- Created basic alerts (e.g., 5 failed logins in 2 minutes).
- All steps are documented with screenshots in `/screenshots/`.

---

## 📎 Notes

- Splunk free license allows indexing of up to 500MB/day (sufficient for testing).
- Time sync across VMs is essential for accurate log correlation.
- Network configured in Bridged mode to allow VMs to reach each other and the host.

---

## 🔗 References

- [Splunk Docs](https://docs.splunk.com/)
- [Sysmon Configs](https://github.com/SwiftOnSecurity/sysmon-config)
- [Auditd Guide](https://linux-audit.com/linux-auditd-tutorial-10-steps-to-basics/)

