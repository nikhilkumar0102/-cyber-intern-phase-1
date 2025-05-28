# Cyber Intern Phase 1

This repository documents the setup of a cybersecurity lab environment for Phase 1 of the Cyber Intern Program. The main goal is to simulate and detect attack scenarios using Splunk Enterprise, Windows 10 with Sysmon, and Linux (Kali) as the attacker machine.

---

## 🛠️ Lab Components

| Component         | Description                                          |
|------------------|------------------------------------------------------|
| VirtualBox        | Virtualization platform for running VMs             |
| Kali Linux        | Attacker machine (Linux)                            |
| Windows 10        | Target machine for attack simulation                |
| Sysmon            | Logs detailed Windows activities                    |
| Splunk Enterprise | SIEM used for log ingestion and analysis            |
| Splunk Forwarder  | Sends logs from Windows 10 to Splunk on Kali Linux  |

---

## 📦 Virtual Machines Used

### 1. 🐧 Kali Linux (Attacker)
- **Tool Installed:** Splunk Enterprise
- **Use:** Hosts the SIEM platform, receives forwarded logs from Windows
- **Resources:** 2 CPU cores, 4 GB RAM, 20+ GB disk

### 2. 🪟 Windows 10 (Victim / Target)
- **Tools Installed:**
  - Sysmon (System Monitor)
  - Splunk Universal Forwarder
- **Use:** Simulate attacks (malware execution, USB insertion, brute-force, etc.)
- **Resources:** 2 CPU cores, 4 GB RAM, 40 GB disk

---

## 🔐 SIEM Setup - Splunk Enterprise on Kali Linux

1. **Download Splunk Enterprise:**
   - [Splunk Download](https://www.splunk.com/en_us/download/splunk-enterprise.html)
  
2. . **Install Splunk:**
   ```bash
   sudo dpkg -i splunk_package_name.deb
   sudo /opt/splunk/bin/splunk start --accept-license
   ```

3. **Set up admin credentials and login to**:
```bash
http://localhost:8000
```

4. **Create a Splunk Index (e.g., `windows`)**

   
## 🚀 Sysmon + Universal Forwarder on Windows 10

1. **Install Sysmon**
- `Download from`:
   - [Sysinternals Sysmon](https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon)

- Install Command:
```powershell
sysmon -accepteula -i sysmonconfig.xml
```
- `Sysmon Config`: Use SwiftOnSecurity community config:
  - [sysmon-config repo](https://github.com/SwiftOnSecurity/sysmon-config)
 
## 🚀 Sysmon + Universal Forwarder on Windows 10

### 1. Install Sysmon
- **Download from:**
   - [Sysinternals Sysmon](https://docs.microsoft.com/en-us/sysinternals/downloads/sysmon)
- **Install Command:**
  ```powershell
  sysmon -accepteula -i sysmonconfig.xml
  ```
- **Sysmon Config**: Use the community config from SwiftOnSecurity:
[](https://github.com/SwiftOnSecurity/sysmon-config)

### 2.  **Install Splunk Universal Forwarder**
-  `Download`: Splunk Universal Forwarder
-  **Install Options**:
   - Use the GUI installer
   - Or use MSI silent install for automation

### 3.  **Configure Forwarder to Send Logs to Splunk**
- Create a custom deployment app on the forwarder:
   - Path: `C:\Program Files\SplunkUniversalForwarder\etc\apps\windows_inputs\`

- Create the inputs.conf file:
```inputs.conf
[WinEventLog://Microsoft-Windows-Sysmon/Operational]
disabled = 0
index = windows_logs
```
- Create the outputs.conf file:
```ini
[tcpout]
defaultGroup = default-autolb-group

[tcpout:default-autolb-group]
server = <KALI_IP>:9997

[tcpout-server://<KALI_IP>:9997]
```
Replace `<KALI_IP>` with the IP address of your Kali Linux (Splunk Enterprise) machine.

### 4. **Restart the Splunk Forwarder Service**
Run the following PowerShell command:

```powershell
Restart-Service splunkforwarder
```
Once complete, you should start seeing logs from Sysmon in your Splunk instance under the index you specified (e.g., `windows`).

## 📁 Repo Structure

cyber-intern-phase-1/
├── logs/
│   └── sample_logs.json
├── screenshots/
│   └── splunk_dashboard.png
├── reports/
│   └── setup_summary.md
├── hints/
│   └── helpful_links.md
├── configs/
│   ├── sysmonconfig.xml
│   ├── inputs.conf
│   └── outputs.conf
└── README.md




   
   
