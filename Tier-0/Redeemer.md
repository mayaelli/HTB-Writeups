# Hack The Box: Redeemer Walkthrough & Key Learnings

## Overview
* **Target IP:** `10.129.163.252`
* **Target Service:** Redis (Port 6379)
* **Final Flag:** `03e1d2b376c37ab3f5319922053953eb`

---

## Technical Confusion & Solutions

### 1. Default Nmap Port Lists
* **Issue:** Running `nmap -sV 10.129.163.252` reported all 1000 ports as closed/ignored.
* **Explanation:** By default, Nmap scans its top 1,000 most common TCP ports. Port `6379` (Redis) is non-standard and not in Nmap's default top-1000 list.
* **Fix:** Explicitly define the port with the `-p` flag:
  ```powershell
  nmap -p 6379 -sV 10.129.163.252

```

### 2. PowerShell PATH & Environment Variables

* **Issue:** Running `nmap` resulted in `CommandNotFoundException` right after installation.
* **Explanation:** PowerShell inherits system environment variables (`PATH`) when launched. If software is installed while PowerShell is open, the current terminal session will not recognize the new executable path.
* **Fix:** Open a new PowerShell window, or dynamically update the environment path in the current session:
```powershell
$env:Path += ";C:\Program Files (x86)\Nmap;C:\Program Files\Nmap"

```



### 3. Redis Commands: `KEYS` vs. `GET`

* **Issue:** Confusion between finding stored data names vs. reading their contents.
* **Explanation:**
* `KEYS *` lists all key identifiers stored in the active database.
* `GET <key_name>` reads the value assigned to that specific key (e.g., `GET flag`).



---

## Task Questions & Answers

| Task # | Question Summary | Answer |
| --- | --- | --- |
| **Task 1** | Which TCP port is open on the machine? | `6379` |
| **Task 2** | Which service is running on the open port? | `redis` |
| **Task 3** | Which command-line utility is used to interact with Redis? | `redis-cli` |
| **Task 4** | Which command is used to obtain info and stats about the server? | `info` |
| **Task 5** | Which command is used to select the desired database in Redis? | `select` |
| **Task 6** | How many keys are present inside the database with index 0? | `4` |
| **Task 7** | Which command is used to obtain all the keys in a database? | `keys *` |
| **Root Flag** | Submit the flag | `03e1d2b376c37ab3f5319922053953eb` |

