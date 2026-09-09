# Hack The Box: Dancing Write-Up

## Machine Overview
* **Name:** Dancing
* **Difficulty:** Very Easy (Tier 0)
* **OS:** Windows
* **Target IP:** `10.129.162.96`
* **Protocols/Services Tested:** SMB (Server Message Block)

---

## 1. What is SMB and What Are We Doing?

**Server Message Block (SMB)** is a networking protocol used by Windows computers to share files, printers, and other resources across a network. 

When a computer runs SMB, it hosts **"Shares"**—which are simply shared folders that other computers on the network can access if given permission.

Our goal for this machine was to:
1. Scan or check what shared folders (shares) the target machine has open.
2. Find a share that allows access without requiring a password (**Anonymous/Guest access**).
3. Look through the files in that shared folder to find `flag.txt`.

---

## 2. Setting Up the VPN Connection

Before interacting with the target, we needed to join Hack The Box's private lab network using **OpenVPN GUI**:

1. Opened **OpenVPN GUI** on Windows.
2. Imported the Hack The Box `.ovpn` configuration file.
3. Connected to the VPN.

### Verifying Connection
To ensure our computer could reach the target machine across the VPN tunnel, we sent a simple network check (**ping**):

```cmd
ping 10.129.162.96
```

**Result:** Received active replies, confirming a direct connection.

---

## 3. Enumerating SMB Shares

Next, we needed to list the available SMB shares on the target machine.

There are **4 default/custom shares** on this machine:
* `ADMIN$` — Administrative share used by remote management tools.
* `C$` — Administrative share mapping to the main operating system drive (`C:\`).
* `IPC$` — Inter-Process Communication share (used for background system communication).
* `WorkShares` — A custom shared folder created on the machine.

---

## 4. Gaining Access & Retrieving the Flag

To access the `WorkShares` folder without needing password authentication, we used Windows credentials caching and drive mapping:

### Step 1: Injecting Guest Credentials
We used `cmdkey` to tell Windows to connect to `10.129.162.96` using an empty password with the `Guest` account:

```cmd
cmdkey /add:10.129.162.96 /user:Guest /pass:""
```

### Step 2: Mapping the Shared Folder as a Local Drive
We mapped the target's `WorkShares` folder as drive letter `Z:` on our local machine using `net use`:

```cmd
net use Z: \\10.129.162.96\WorkShares
```

### Step 3: Navigating to the Flag
Once mapped as drive `Z:`, we switched into that drive, navigated into James's folder, and viewed the flag file:

```cmd
Z:
cd James.P
type flag.txt
```

**Flag Captured:** `5f61c10dffbc77a704d76016a22f1664`

---

## 5. Cleaning Up

After securing the flag, we safely unmapped the network drive and cleared saved credentials:

```cmd
C:
net use Z: /delete
cmdkey /delete:10.129.162.96
```

---

## Key Command Cheat Sheet

| Command | What it does in simple terms |
| :--- | :--- |
| `ping <IP>` | Checks if another computer on the network is online and reachable. |
| `cmdkey /add:...` | Remembers a username/password for a specific IP address so you don't get prompted. |
| `net use Z: \\<IP>\<Share>` | Connects a shared network folder and makes it act like a drive (`Z:`) on your PC. |
| `Z:` | Switches your terminal's current location from drive `C:` to drive `Z:`. |
| `cd <Folder>` | **C**hange **D**irectory — moves inside a specific folder. |
| `type <File>` | Displays the contents of a text file on your screen (equivalent to `cat` in Linux). |
| `net use Z: /delete` | Disconnects and removes the mapped network drive. |

---

## Key Takeaways
* SMB shares often hold sensitive data if improper permissions are configured.
* Public or internal SMB shares should never allow unauthenticated (`Guest`/blank password) read or write permissions.
* Mapping remote network folders locally provides a simple way to inspect target file systems during security audits.
