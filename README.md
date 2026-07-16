<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 h2 { margin-top: 0.35cm; margin-bottom: 0.21cm; background: transparent; page-break-after: avoid }
 h2.western { font-weight: bold; font-size: 18pt; font-family: "Liberation Serif", serif }
 h2.cjk { font-weight: bold; font-size: 18pt; font-family: "Noto Serif CJK SC" }
 h2.ctl { font-weight: bold; font-family: "Noto Sans Devanagari"; font-size: 18pt }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 strong { font-weight: bold }
 </style>

<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 h1 { margin-bottom: 0.21cm; background: transparent; page-break-after: avoid }
 h1.western { font-weight: bold; font-size: 24pt; font-family: "Liberation Serif", serif }
 h1.cjk { font-weight: bold; font-size: 24pt; font-family: "Noto Serif CJK SC" }
 h1.ctl { font-weight: bold; font-family: "Noto Sans Devanagari"; font-size: 24pt }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 strong { font-weight: bold }
 a:link { color: #000080; text-decoration: underline }
 a:visited { color: #800000; text-decoration: underline }
 </style>

# Agentic SOC Lab — Network‑Focused (Zeek‑Only Phase)

A lightweight, modular, Security‑Onion‑inspired SOC
lab designed for learning real SOC analyst skills on constrained
hardware. This project replicates the **network‑visibility
workflow** used in enterprise SOCs — without requiring the
full Security Onion appliance.

The lab is built on **KVM/libvirt**, uses **Zeek** for deep protocol analysis, and integrates an **AI‑assisted
incident reporting pipeline** to generate professional SOC
documentation.

 🌐 Network Topology

The diagram below illustrates
the **Agentic SOC‑Lab network architecture**,
designed for safe, isolated threat‑detection experiments within
a KVM/libvirt environment. Each layer represents a distinct boundary
of control, ensuring realistic traffic flow while maintaining
complete separation from the home LAN.

<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 h3 { margin-top: 0.25cm; margin-bottom: 0.21cm; background: transparent; page-break-after: avoid }
 h3.western { font-weight: bold; font-size: 14pt; font-family: "Liberation Serif", serif }
 h3.cjk { font-weight: bold; font-size: 14pt; font-family: "Noto Serif CJK SC" }
 h3.ctl { font-weight: bold; font-family: "Noto Sans Devanagari"; font-size: 14pt }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 strong { font-weight: bold }
 a:link { color: #000080; text-decoration: underline }
 a:visited { color: #800000; text-decoration: underline }
 </style>

### 1. Internet & Host

Layer**

- **ISP
   Router** — Provides external connectivity to the host
   system.

- **Host
   (EndeavourOS)** — The physical machine running KVM/libvirt.
  
  - Acts
     as the hypervisor and management console.
  
  - No
     direct network bridge to attacker or victim VMs.
  
  - Ensures
     isolation between lab and home network.

### **2. Virtualization Layer**

- **KVM
   / Libvirt** — Manages all virtual machines and defines
   internal virtual networks.

- **WAN
   Interface** — Serves as the logical entry point into the
   lab’s isolated environment.

- **Router
   & Firewall** — Implements segmentation between external
   and internal traffic.
  
  - Prevents
     attacker VMs from reaching the host or home LAN.
  
  - Routes
     packets through the inline Zeek sensor for inspection.

### **3. Internal SOC‑LAN

Segment**

This is the operational heart
of the lab — a fully isolated network where attack simulations and
detections occur.

<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 th p { widows: 0; text-align: center; orphans: 0; font-weight: bold; background: transparent }
 td p { widows: 0; orphans: 0; background: transparent }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 strong { font-weight: bold }
 a:link { color: #000080; text-decoration: underline }
 a:visited { color: #800000; text-decoration: underline }
 </style>

| Role              | VM            | OS                   | Function                        |
| ----------------- | ------------- | -------------------- | ------------------------------- |
| **Attacker #1**   | Parrot Linux  | Offensive simulation | Generates exploit traffic       |
| **Attacker #2**   | Void Linux    | Offensive simulation | Alternative attack source       |
| **Inline Sensor** | Zeek          | Ubuntu Server        | Captures and parses all traffic |
| **Target #1**     | Windows 10    | Victim endpoint      | Receives attack payloads        |
| **Target #2**     | Ubuntu Server | Victim endpoint      | Hosts vulnerable services       |

<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 code.western { font-family: "Liberation Mono", monospace }
 code.cjk { font-family: "Noto Sans Mono CJK SC", monospace }
 code.ctl { font-family: "Liberation Mono", monospace }
 strong { font-weight: bold }
 a:link { color: #000080; text-decoration: underline }
 a:visited { color: #800000; text-decoration: underline }
 </style>

Traffic flow: `Attacker(s) → Inline Sensor (Zeek) → Targets`

This inline configuration
ensures **complete packet visibility** for Zeek while
maintaining realistic routing behaviour.

<title></title>

<style type="text/css">
 @page { size: 21cm 29.7cm; margin: 2cm }
 h3 { margin-top: 0.25cm; margin-bottom: 0.21cm; background: transparent; page-break-after: avoid }
 h3.western { font-weight: bold; font-size: 14pt; font-family: "Liberation Serif", serif }
 h3.cjk { font-weight: bold; font-size: 14pt; font-family: "Noto Serif CJK SC" }
 h3.ctl { font-weight: bold; font-family: "Noto Sans Devanagari"; font-size: 14pt }
 p { margin-bottom: 0.25cm; line-height: 115%; background: transparent }
 code.western { font-family: "Liberation Mono", monospace }
 code.cjk { font-family: "Noto Sans Mono CJK SC", monospace }
 code.ctl { font-family: "Liberation Mono", monospace }
 strong { font-weight: bold }
 a:link { color: #000080; text-decoration: underline }
 a:visited { color: #800000; text-decoration: underline }
 </style>

### **4. Log & Analysis

Flow**

- Zeek
   generates structured logs (`conn.log`,
   `dns.log`, `http.log`,
   etc.).

- Logs
   are forwarded to the **Analysis VM** for indexing and
   visualization.

- The
   AI summarizer processes these logs to produce incident timelines and
   reports.

- Reports
   are stored under `/opt/agentic-soc/reports/` and committed to GitHub for portfolio documentation.

### **5. Security &

Isolation Principles**

- No VM
   is bridged to the physical LAN.

- All
   traffic remains within the virtual SOC‑LAN.

- The
   inline sensor acts as both a **firewall** and
   **visibility node**.

- The
   design mirrors enterprise SOC segmentation:
  
  - External
     → DMZ → Internal → Monitoring.

### **6. Future Expansion**

This topology supports
modular growth:

- Add
   **Suricata IDS** beside Zeek for signature‑based
   detection.

- Introduce
   **Host telemetry** (Sysmon, Wazuh, Elastic Agent).

- Deploy
   **Centralized log aggregation** for multi‑sensor
   scaling.

- Integrate
   **AI‑driven correlation** for automated threat
   summarization.

![Project Architecture Diagram](images/CybrSEC_KVMlab.drawio.svg)

'''

flowchart TD

    %% External Layer
    INTERNET(["🌐 Internet"])
    ISP(["🏠 ISP Router"])
    HOST(["💻 Host System (EndeavourOS)"])
    
    %% Virtualization Layer
    KVM(["🧩 KVM / Libvirt Virtualization Layer"])
    WAN(["🌐 Virtual WAN Interface"])
    FW(["🛡️ Virtual Router / Firewall"])
    
    %% SOC-LAN Label
    subgraph SOC_LAN ["🔒 SOC-LAN (Isolated VLAN)"]
        ATT1(["⚔️ Parrot Attacker"])
        ATT2(["⚔️ Void Attacker"])
        SENSOR(["🧅 Inline Sensor (Zeek)"])
        WIN(["🪟 Windows 10 Target"])
        UBUNTU(["🐧 Ubuntu Server Target"])
    end
    
    %% Analysis Layer
    ANALYSIS(["📊 Analysis VM (OpenSearch / Grafana Loki)"])
    AI(["🤖 AI Summarizer"])
    GITHUB(["📁 GitHub Documentation"])
    
    %% Management Network (Optional)
    subgraph MGMT ["🔧 Management Network (SSH / Console Access)"]
        HOST_MGMT(["Host SSH / Console"])
        SENSOR_MGMT(["Sensor SSH"])
        ANALYSIS_MGMT(["Analysis SSH"])
        WIN_MGMT(["Windows RDP/SSH"])
        UBUNTU_MGMT(["Ubuntu SSH"])
    end
    
    %% External Flow
    INTERNET --> ISP --> HOST --> KVM --> WAN --> FW
    
    %% SOC-LAN Flow
    FW --> ATT1
    FW --> ATT2
    
    ATT1 --> SENSOR
    ATT2 --> SENSOR
    
    SENSOR --> WIN
    SENSOR --> UBUNTU
    
    %% Log Forwarding
    SENSOR -- "Zeek Logs → Filebeat/rsyslog" --> ANALYSIS
    
    %% Reporting Workflow
    ANALYSIS --> AI --> GITHUB
    
    %% Management Network Connections
    HOST_MGMT -.-> SENSOR_MGMT
    HOST_MGMT -.-> ANALYSIS_MGMT
    HOST_MGMT -.-> WIN_MGMT
    HOST_MGMT -.-> UBUNTU_MGMT

'''
