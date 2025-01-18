## **Logs and Analysis Commands : secure-20240804.txt**

### **Extract Failed Password Attempts**
```bash
[madhu@localhost logs]$ grep "Failed password" secure-20240804 > failed_attempts_1.txt
[madhu@localhost logs]$ grep "Failed password" secure-20240804 | awk '{print $9}' | sort | uniq -c > usernames_1.txt
[madhu@localhost logs]$ grep "Failed password" secure-20240804 | awk '{print $11}' | sort | uniq -c > ip_addresses_1.txt
```

### **Top Usernames and IP Addresses**
```bash
[madhu@localhost logs]$ cat usernames_1.txt | sort -nr | head
  23224 root
    447 invalid
      7 mysql
      2 sshd
      1 operator
      1 adm
[madhu@localhost logs]$ cat ip_addresses_1.txt | sort -nr | head
  15579 221.122.88.121
   5126 219.159.100.72
    958 223.82.216.42
    416 167.172.79.30
    111 183.81.169.238
     75 admin
     45 80.94.95.81
     42 ubuntu
     25 190.104.25.210
     25 103.7.226.128
```

## **Logs and Analysis Commands : secure.txt**

### **Extract Failed Password Attempts**
```bash
[madhu@localhost logs]$ grep "Failed password" secure > failed_attempts_2.txt
[madhu@localhost logs]$ grep "Failed password" secure | awk '{print $9}' | sort | uniq -c > usernames_2.txt
[madhu@localhost logs]$ grep "Failed password" secure | awk '{print $11}' | sort | uniq -c > ip_addresses_2.txt
```

### **Top Usernames and IP Addresses**
```bash
[madhu@localhost logs]$ cat usernames_2.txt | sort -nr | head
   2807 invalid
   2690 root
     19 www
     15 ftp
     10 mysql
      8 sshd
      7 operator
      6 mail
      6 apache
      5 bin
[madhu@localhost logs]$ cat ip_addresses_2.txt | sort -nr | head
    868 101.126.66.68
    230 183.81.169.238
    175 185.234.216.97
    167 admin
    105 143.110.246.247
    100 83.222.191.62
     88 170.64.139.190
     85 101.126.4.215
     52 user
     46 218.92.0.59
```

### **Filter valid users**
```bash
[madhu@localhost logs]$ echo -e "user1\nuser2\nroot\nadmin" > valid_users.txt
[madhu@localhost logs]$ grep -f valid_users.txt failed_attempts_1.txt > valid_users_attacked1.txt
[madhu@localhost logs]$ grep -f valid_users.txt failed_attempts_2.txt > valid_users_attacked2.txt
```

## **Security Enhancements**

### **Editing SSH Configuration**
```
[madhu@localhost logs]$ sudo vi /etc/ssh/sshd_config
```

### **Installing EPEL Release**
```
madhu@localhost logs]$ sudo yum install epel-release -y
```
Last metadata expiration check: 0:23:11 ago on Sat 18 Jan 2025 10:56:30 AM IST.

Running transaction
  Preparing        :                                                        1/1
  Installing       : epel-release-9-7.el9.noarch                            1/2
  Running scriptlet: epel-release-9-7.el9.noarch                            1/2
Many EPEL packages require the CodeReady Builder (CRB) repository.
It is recommended that you run /usr/bin/crb enable to enable the CRB repository.

  Installing       : epel-next-release-9-7.el9.noarch                       2/2
  Running scriptlet: epel-next-release-9-7.el9.noarch                       2/2
  Verifying        : epel-next-release-9-7.el9.noarch                       1/2
  Verifying        : epel-release-9-7.el9.noarch                            2/2

Installed:
  epel-next-release-9-7.el9.noarch          epel-release-9-7.el9.noarch        

Complete!


### **Installing Fail2Ban**
```
[madhu@localhost logs]$ sudo yum install fail2ban -y
```
Running transaction
  Preparing        :                                                        1/1
  Installing       : libesmtp-1.0.6-24.el9.x86_64                           1/8
  Running scriptlet: fail2ban-selinux-1.0.2-12.el9.noarch                   2/8
  Installing       : fail2ban-selinux-1.0.2-12.el9.noarch                   2/8
  Running scriptlet: fail2ban-selinux-1.0.2-12.el9.noarch                   2/8
libsemanage.semanage_direct_install_info: Overriding fail2ban module at lower priority 100 with module at priority 200.

  Installing       : fail2ban-server-1.0.2-12.el9.noarch                    3/8
  Running scriptlet: fail2ban-server-1.0.2-12.el9.noarch                    3/8
  Installing       : fail2ban-firewalld-1.0.2-12.el9.noarch                 4/8
  Installing       : liblockfile-1.14-10.el9.x86_64                         5/8
  Installing       : esmtp-1.2-19.el9.x86_64                                6/8
  Running scriptlet: esmtp-1.2-19.el9.x86_64                                6/8
  Installing       : fail2ban-sendmail-1.0.2-12.el9.noarch                  7/8
  Installing       : fail2ban-1.0.2-12.el9.noarch                           8/8
  Running scriptlet: fail2ban-selinux-1.0.2-12.el9.noarch                   8/8
  Running scriptlet: fail2ban-1.0.2-12.el9.noarch                           8/8
  Verifying        : liblockfile-1.14-10.el9.x86_64                         1/8
  Verifying        : esmtp-1.2-19.el9.x86_64                                2/8
  Verifying        : fail2ban-1.0.2-12.el9.noarch                           3/8
  Verifying        : fail2ban-firewalld-1.0.2-12.el9.noarch                 4/8
  Verifying        : fail2ban-selinux-1.0.2-12.el9.noarch                   5/8
  Verifying        : fail2ban-sendmail-1.0.2-12.el9.noarch                  6/8
  Verifying        : fail2ban-server-1.0.2-12.el9.noarch                    7/8
  Verifying        : libesmtp-1.0.6-24.el9.x86_64                           8/8

Installed:
  esmtp-1.2-19.el9.x86_64                 fail2ban-1.0.2-12.el9.noarch        
  fail2ban-firewalld-1.0.2-12.el9.noarch  fail2ban-selinux-1.0.2-12.el9.noarch
  fail2ban-sendmail-1.0.2-12.el9.noarch   fail2ban-server-1.0.2-12.el9.noarch  
  libesmtp-1.0.6-24.el9.x86_64            liblockfile-1.14-10.el9.x86_64      

Complete!


### **Enabling and Starting Fail2Ban Service**
```
[madhu@localhost logs]$ sudo systemctl enable fail2ban
Created symlink /etc/systemd/system/multi-user.target.wants/fail2ban.service → /usr/lib/systemd/system/fail2ban.service.
[madhu@localhost logs]$ sudo systemctl start fail2ban
```

### **Editing Password Quality Configuration**
```
password: sudo vi /etc/security/pwquality
```

### **Installing Google Authenticator**
```
[madhu@localhost logs]$ sudo yum install google-authenticator -y
```
[sudo] password for madhu:
Last metadata expiration check: 0:12:37 ago on Sat 18 Jan 2025 11:21:31 AM IST.

Running transaction
  Preparing        :                                                        1/1
  Installing       : google-authenticator-1.09-5.el9.x86_64                 1/1
  Running scriptlet: google-authenticator-1.09-5.el9.x86_64                 1/1
  Verifying        : google-authenticator-1.09-5.el9.x86_64                 1/1

Installed:
  google-authenticator-1.09-5.el9.x86_64                                        

Complete!

### **Aunthenticator Setup**
``` 
[madhu@localhost logs]$ google-authenticator

Do you want authentication tokens to be time-based (y/n) y
Warning: pasting the following URL into your browser exposes the OTP secret to Google:
  https://www.google.com/chart?chs=200x200&chld=M|0&cht=qr&chl=otpauth://totp/madhu@localhost.localdomain%3Fsecret%3DXPHKKNSPZBJI7UKSJY65I2PY5Y%26issuer%3Dlocalhost.localdomain
Failed to use libqrencode to show QR code visually for scanning.
Consider typing the OTP secret into your app manually.
Your new secret key is: XPHKKNSPZBJI7UKSJY65I2PY5Y
Enter code from app (-1 to skip): -1
Code confirmation skipped
Your emergency scratch codes are:
  45659018
  12165235
  45975935
  11809742
  41521415

Do you want me to update your "/home/madhu/.google_authenticator" file? (y/n) y

Do you want to disallow multiple uses of the same authentication
token? This restricts you to one login about every 30s, but it increases
your chances to notice or even prevent man-in-the-middle attacks (y/n) y

By default, a new token is generated every 30 seconds by the mobile app.
In order to compensate for possible time-skew between the client and the server,
we allow an extra token before and after the current time. This allows for a
time skew of up to 30 seconds between authentication server and client. If you
experience problems with poor time synchronization, you can increase the window
from its default size of 3 permitted codes (one previous code, the current
code, the next code) to 17 permitted codes (the 8 previous codes, the current
code, and the 8 next codes). This will permit for a time skew of up to 4 minutes
between client and server.
Do you want to do so? (y/n) y

If the computer that you are logging into isn't hardened against brute-force
login attempts, you can enable rate-limiting for the authentication module.
By default, this limits attackers to no more than 3 login attempts every 30s.
Do you want to enable rate-limiting? (y/n) y
```

## **Continuous Monitoring and Reporting**

### **Nagios Installation**
```
[madhu@localhost logs]$ sudo yum  install nagios -y
```

### **Nagios Configuration Validation and Alert Setup**
```
[madhu@localhost logs]$ ls /etc/nagios/nagios.cfg
etc/nagios/nagios.cfg
[madhu@localhost logs]$ sudo nagios -v /etc/nagios/nagios.cfg
[sudo] password for madhu:
```
