# 🎯 Hack The Box: Meow Write-up

---

## 📌 Machine Overview

| Parameter | Value |
| :--- | :--- |
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

First, we verified basic network connectivity to the target machine using `ping`:

```bash
ping -c 4 <TARGET_IP>
