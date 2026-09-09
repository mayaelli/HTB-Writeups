# 🎯 Hack The Box: Fawn Write-up

---

## 📌 Machine Overview

| Parameter | Value |
| :--- | :--- |
| **Machine Name** | Fawn |
| **OS** | Unix / Linux |
| **Difficulty** | Very Easy |
| **Target IP** | `10.129.162.236` |
| **Primary Service** | FTP (`vsftpd 3.0.3`) |
| **Port** | `21/TCP` |
| **Vulnerability** | Anonymous Authentication Enabled |
| **Root Flag** | `035db21c881520061c53e0536e44f815` |

---

## 🔍 Step-by-Step Execution Log

### 1. Service Enumeration (`Nmap`)

We started by identifying open ports and active service versions on the target machine using `nmap`:

```bash
nmap -sV 10.129.162.236
