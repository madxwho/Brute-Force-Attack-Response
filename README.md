# Brute-Force-Attack-Response

This repository contains the implementation and documentation for analyzing and responding to brute force attacks on a CentOS server. The project includes a comprehensive log analysis and step-by-step mitigation strategies to enhance server security.

---

## Introduction

Brute force attacks are common cybersecurity threats where an attacker attempts to guess a username and password combination to gain unauthorized access to a system. This project demonstrates how to detect such attacks through log analysis, secure the server, and prevent future occurrences using tools like Fail2Ban, SSH hardening, and advanced monitoring.

---

## Project Structure
- **`README.md`**: Overview and instructions for the project.
- **`commands.md`**: A detailed list of commands used in the analysis and mitigation process.
- **`secure-logs_1733594589588.zip`**: A compressed file containing the server logs.
- **`Screenshots/`**: Folder containing screenshots of the project in action.

---

## Setup Instructions

1. **Install CentOS 9 on VirtualBox**
   - Download the CentOS 9 ISO and create a virtual machine in VirtualBox.
   - Configure network settings for the virtual machine.

2. **Enable SSH Access**
   - Open and edit the SSH configuration:
     ```bash
     sudo vi /etc/ssh/sshd_config
     ```
   - Uncomment the `Port` line and set it to a custom value (e.g., 2222).
   - Restart the SSH service:
     ```bash
     sudo systemctl restart sshd
     ```

3. **Transfer Log Files**
   - Use shared folders or `scp` to transfer files between the host machine and the virtual machine.

4. **Analyze Log Files**
   - Use tools like `grep` and `awk` to analyze the log files for suspicious activity.

5. **Install Security Tools**
   - Install `fail2ban` and configure it for brute force attack prevention:
     ```bash
     sudo yum install fail2ban
     ```

---

## Steps to Analyze and Mitigate Attacks

1. **Log Analysis**
   - Analyze the authentication logs:
     ```bash
     cat /var/log/secure | grep "authentication failure"
     ```
   - Extract unique IP addresses:
     ```bash
     grep "authentication failure" /var/log/secure | awk '{print $NF}' | sort | uniq
     ```

2. **Implement Security Enhancements**
   - Block IPs using `iptables`:
     ```bash
     sudo iptables -A INPUT -s <IP_ADDRESS> -j DROP
     ```
   - Configure `fail2ban` to automate IP blocking.

3. **Enable Multi-Factor Authentication (MFA)**
   - Install and configure Google Authenticator or similar tools for MFA.

4. **Monitor with Nagios or Zabbix**
   - Set up monitoring tools to track suspicious activity in real time.

---

## Commands Used

Refer to the [`commands.md`](commands.md) file for the complete list of commands and explanations used during this project.

---

## Results and Findings

The analysis revealed multiple brute force attack attempts originating from various IP addresses. These IPs were blocked, and server security was enhanced by implementing:
- Fail2Ban for automated IP blocking.
- SSH hardening techniques such as changing the default port and disabling password authentication.

---

## Screenshots

Refer to the `Screenshots/` folder for visual documentation of the process.

---


