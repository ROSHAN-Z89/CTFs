Browser used: FireFox

The site had a Login Page 
Creds given by Pico: 
	email: ctf-player@picoctf.org
	password:
Viewed the page source:
	Found :
		"
			 <!-- ABGR: Wnpx - grzcbenel olcnff: hfr urnqre "K-Qri-Npprff: lrf" -->
			 <!-- Remove before pushing to production! -->   
		"
	The above is a ROT13 encoded Text 
		ROT13 decoded: "NOTE: Jack - temporary bypass: use header "X-Dev-Access: yes"

Bsically it is saying to use a header to get the flag

Step1: Opened the network tab in the site   
Step2: Reloaded the site
Step3: Tried to login using the email and a random pass 
Step4: Headed towards the login header, It had a 401 status code
Step5: In the header again resended the request, at the left button saw the headers being sent
Step6: scrolled to the last and added the header "X-Dev-Access: yes" and send the request.
Step7: Saw the status code, it was 200 OK
Step8: Went to the Response tab, found the flag and other details.
	{
		"success": true,
		"email": "ctf-player@picoctf.org",
		"firstName": "pico",
		"lastName": "player",
		"flag": "picoCTF{brut4_f0rc4_83812a02}"
	}

