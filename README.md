# EJDER#AV - Threat Intelligence & Heuristic Analysis Tool

EJDER#AV is a lightweight, multi-threaded Python3 utility designed for SOC (Security Operations Center) analysts, incident responders, and security researchers. It automates threat intelligence gathering, file integrity verification, bulk directory scanning, and external network/IP reputation lookups by integrating multiple open-source feeds and premium security APIs.

---

##  Key Features

* **File Analysis:** Calculates cryptographic hashes (MD5, SHA1, SHA256) and correlates them with global malware repositories. Includes a lightweight local **heuristic engine** to flag dangerous extensions, double extensions, hidden files, or embedded suspicious shell commands.
* **IP & Host Reputation:** Resolves hostnames and queries external IPs against continuous threat feeds (**Feodo Tracker C2, FireHOL Level-1, CINS Score**) and platform APIs (**Shodan & AbuseIPDB**). Performs reverse DNS lookup and localized port scanning for common risky protocols (e.g., RDP, SMB, Telnet).
* **Bulk Directory Scanning:** Recursively traverses directories, hashes discovered files asynchronously, and runs parallel API lookups to identify hidden indicators of compromise (IoCs) within a workspace.
* **Direct Hash Querying:** Performs fast lookups for standalone MD5 or SHA256 signatures.
* **Persistent Connection Management:** Utilizes customized HTTP pooling with automatic retry logic (`urllib3.util.retry`) to handle API rate limiting (`HTTP 429`) and gateway timeouts.

---

## Integrated Sources & APIs
| Note: The application is currently configured with valid API keys. These keys can be modified or replaced with your own credentials at any time.
| Source / Platform | API Type | Header / Authentication Required | Purpose |
| :--- | :--- | :--- | :--- |
| **ThreatFox (abuse.ch)** | JSON POST | `Auth-Key` (Required) | Malware IoC & Hash Correlation |
| **MalwareBazaar (abuse.ch)** | Form POST | `Auth-Key` (Required) | Malware Signature & Sample Intelligence |
| **Shodan** | GET Request | `key` Query Parameter | Port/Vulnerability Tracking & OSINT |
| **AbuseIPDB** | GET Request | `Key` Header | IP Maliciousness Confidence Score |
| **Feodo Tracker** | Live Feed | Anonymous / No Key | C2 Infrastructure Tracking |
| **FireHOL (Level-1)** | Live Feed | Anonymous / No Key | Cybercrime & IP Blocklists |
| **CINS Score** | Live Feed | Anonymous / No Key | Malicious Actor IP Reputations |

---

## 📋 Prerequisites & Installation

The tool is standalone and depends primarily on Python's native standard library, with the exception of the `requests` library used for advanced session handling.

1. **Clone or Download the Script:**
   ```bash
   git clone [https://github.com/yourusername/ejder-av.git](https://github.com/yourusername/ejder-av.git)
   cd ejder-av
   chmod +x ejder_av.py
2. CLI Automation Mode

You can bypass the menu by parsing arguments directly:

Scan a single file:

    python3 ejder_av.py -f /path/to/suspicious_file.exe

Query a specific IP or Hostname:

    python3 ejder_av.py -i 8.8.8.8

Perform bulk scanning on a directory recursively:

    python3 ejder_av.py -d /home/user/Downloads

Query a standalone SHA256/MD5 hash:

    python3 ejder_av.py -H e684c5aa42e21bc9c811347071fdf06c59b64dc4f9408b0493392330a84d4b1a

Pass API keys via command line (One-time or Initialization):

    python3 ejder_av.py --threatfox-key YOUR_KEY --malwarebazaar-key YOUR_KEY
