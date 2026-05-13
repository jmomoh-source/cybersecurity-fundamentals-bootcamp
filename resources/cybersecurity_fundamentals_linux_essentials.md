# Cybersecurity Fundamentals & Linux Essentials

---

## Part I: Foundations

### 🔑 Core Security Models
- **CIA Triad (Defensive Goals):**
  - Confidentiality — restrict access to sensitive data.
  - Integrity — ensure accuracy and trustworthiness of data.
  - Availability — keep systems accessible when needed.

- **Attacker’s Objective (DAD):**
  - Disclosure — stealing information.
  - Alteration — tampering with data.
  - Destruction — deleting or blocking access.

- **Triple AAA Framework:**
  - Authentication — verify identity.
  - Authorization — define permissions.
  - Accounting — log and audit actions.

### 🛡️ Risk & Defense
- **Risk Terms:**
  - Vulnerability = flaw.
  - Threat = potential danger.
  - Exploit = method of attack.
  - Exposure = being subject to threat.
  - Impact = measure of loss.

- **Risk Management Strategies:**
  - Treat (patch, firewall).
  - Transfer (insurance).
  - Avoid (stop risky activity).
  - Accept (cost-benefit decision).
  - Nonrepudiation (logging + cryptography).

- **Defense-in-Depth Layers:**
  - Physical → locks, guards.
  - Network → segmentation, firewalls.
  - Host → OS hardening, EDR.
  - Application → input validation.
  - Data → encryption.

### 🔒 Access Control & Hardening
- **Models:**
  - DAC — owner decides (Linux default).
  - RBAC — role-based permissions.
  - MAC — strict system-enforced labels.

- **Principle of Least Privilege (PoLP).**
- **System Hardening:**
  - Disable unused ports/services.
  - Remove unnecessary software.
  - Secure defaults.
  - Enforce strong access controls.

---

## Part II: Linux CLI Essentials

### 📂 Navigation Commands
- `pwd` → show current directory.
- `ls -la` → list files + permissions.
- `cd` → change directory.
- `mkdir` → create folder.
- `touch` → create file.
- `rm` → delete file.

**Example:**
```bash
mkdir bootcamp
cd bootcamp
touch target.txt
ls
```

### 🔧 Pipes & Permissions
- Use `|` and `grep` to filter logs:
```bash
cat sys.log | grep "FAIL"
```

- Change file permissions:
```bash
chmod 777 target.txt
```

- Permission formula:  
  `4 (read) + 2 (write) + 1 (execute) = 7`

---

## 📚 References
- CIA Triad Graphic — medium.com  
- Biometrics Graphic — thefintechtimes.com  
- Defense-in-Depth Diagram — cisotimes.com  
- Linux Terminal Basics — freecodecamp.org  

---

## ✅ Key Takeaways
- Security is built on **CIA Triad**; attackers aim for **DAD**.  
- Risk management requires layered strategies (Treat, Transfer, Avoid, Accept).  
- **Defense-in-Depth** means multiple overlapping controls.  
- Linux CLI mastery is essential for navigation, permissions, and system hardening.  