
<h1>Dig Dug</h1>  
<b>Category</b>: Web Warm-up
<h2>Description:</h2>

The pot calling the kettle black. 

the link provided https://digx.asisctf.com/  is a static page with image included (dig-tool.jpg) 
with a text :
```
If you want to know what is dig, you should consider that dig stands for domain information groper.

Thank you for seeing our digs.
```

so the idea is to use Domain Information Groper (DIG) i used dnsdumpster which an online DNS Digger (service) 
```
https://dnsdumpster.com/ 
```

the querry answer was an url in the A record 
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Dig%20Dug/image/f1.PNG)


which is 
```
https://airplane.asisctf.com/ 
```

after visiting the URL we find a simple message :
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Dig%20Dug/image/f2.PNG)

and in the source code we can see a JS file included (but obfuscated) 

![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Dig%20Dug/image/f3.PNG)


so all we have to do is to decode the JS and we search for ASIS{ 

and the flag was :
![alt text](https://raw.githubusercontent.com/occupe/Writeups-CTF/master/ASIS-CTF/Dig%20Dug/image/ff.PNG)
