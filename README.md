# Kali_Linux_MCP

## Overview
Kali_Linux_MCP exposes Kali tools through:
- **Kali_Linux_Server.py**: Flask API wrapping tools like `nmap`, `gobuster`, `nikto`, `sqlmap`, `metasploit`, `hydra`, `john`, `wpscan`, `enum4linux`.  
- **MCP_Server.py**: MCP bridge using FastMCP, forwarding requests from MCP clients to the API.

Use it for **authorized labs, CTFs, HTB/THM machines**, or AI-assisted testing via MCP clients (Claude Desktop, 5ire, etc.).

---

## Requirements
- Kali Linux (or Linux with tools installed in PATH).  
- Python 3 with `flask`, `requests`, `mcp`.  
- Install:  
  pip install flask requests mcp

---

## Run

1. Start API:  
   python3 Kali_Linux_Server.py --port 5000  

2. Health check:  
   curl http://localhost:5000/health  

3. Start MCP bridge:  
   python3 MCP_Server.py --server http://localhost:5000 --timeout 300  

---

## API Endpoints
- GET `/health` — tool status.  
- POST `/api/command` — run any command.  
- POST `/api/tools/<tool>` — wrappers for nmap, gobuster, dirb, nikto, sqlmap, metasploit, hydra, john, wpscan, enum4linux.  

Each requires JSON body with tool-specific args (`target`, `url`, etc.).

---

## MCP Tools
Bridge registers MCP tools: `nmap_scan`, `gobuster_scan`, `dirb_scan`, `nikto_scan`, `sqlmap_scan`, `metasploit_run`, `hydra_attack`, `john_crack`, `wpscan_analyze`, `enum4linux_scan`, plus `execute_command` and `check_health`.

---

## Example Usage
- Nmap:  
  curl -X POST http://localhost:5000/api/tools/nmap -H "Content-Type: application/json" -d '{"target":"scanme.nmap.org","additional_args":"-sV"}'  

- WPScan:  
  curl -X POST http://localhost:5000/api/tools/wpscan -H "Content-Type: application/json" -d '{"url":"https://example.com","additional_args":"--enumerate u"}'  

- MCP Client:  
  Add MCP config pointing `python3 /path/to/MCP_Server.py --server http://LINUX_IP:5000`.

---

## Scenarios
- Recon: `nmap_scan` to map services.  
- Web enum: `gobuster_scan` or `dirb_scan`.  
- Vuln triage: `nikto_scan`, `sqlmap_scan`.  
- WordPress checks: `wpscan_analyze`.  
- SMB recon: `enum4linux_scan`.  
- Credential tests (lab only): `hydra_attack`, `john_crack`.  
- Exploit check: `metasploit_run`.  

---

## Notes
- Supports AI-assisted workflows: models suggest and run commands.  
- Works with Claude Desktop, 5ire MCP clients.  
- Extendable: other forensic tools (Volatility, SleuthKit) possible.  

---

## Security
- Executes system commands directly. Do not expose to untrusted networks.  
- Use only in labs or with explicit permission.  

---
