doing some recon with nmap :
i notive an open DNS port (53)
so i type : 
dig -x 10.13.37.10 @10.13.37.10 and get this back 
![[Pasted image 20240406171323.png]]

there is a website connected to the same ip address which acts as a domain name server 
Second flag found : 
![[Pasted image 20240406171423.png]]

Found a suspecious js file : 
![[Pasted image 20240406171451.png]]

i found a js script and executed it online which lead me to a secret directory ! 
![[Pasted image 20240406171554.png]]

FOUND a admin login panel :
![[Pasted image 20240406171713.png]]

tried couple of stuff and apparently its vulnerable to blind sql injection i used this payload to check : `0'XOR(if(now()=sysdate(),sleep(5),0))XOR'Z`
Great payloads list : [Link](https://ansar0047.medium.com/blind-sql-injection-detection-and-exploitation-cheatsheet-17995a98fed1)
command : `sqlmap -r req --batch --level=5 --risk 3 --dump --dbs`
so i used sqlmap to dump the databases and the tables :
![[Pasted image 20240406172048.png]]



![[Pasted image 20240406172133.png]]

it appears to be a sha256 hash chatGPT helped me figure it out.
anyway i cracked it with hashcat :

![[Pasted image 20240406172235.png]]


Found an XSS vuln but honestly i couldnt do much with it for the moment :
whenever i send a swearword the email.php file changes it :
![[Pasted image 20240406172701.png]]

in php this is known as preg_replace() function after some research i found an exploit for it:
after a long night i came up to this :
![[Pasted image 20240406173510.png]]

now i dont really know why exactly it only worked for swearwords[/fuck/e]=system('id')
![[Pasted image 20240406173616.png]]

But his article explains it best :
https://isharaabeythissa.medium.com/command-injection-preg-replace-php-function-exploit-fdf987f767df

now for the reverse shell part : 
Command : 

![[Pasted image 20240406173944.png]]


Listener

![[Pasted image 20240406173915.png]]

Enviroment setup : 
![[Pasted image 20240406174105.png]]

another flag :
![[Pasted image 20240406174152.png]]


users found : ![[Pasted image 20240406174542.png]]

inside of db.php
$servername = "localhost";
$username = "jet";
$password = "dcr46kdl6zsld68idtyufldro";

// Create connection
$conn = new mysqli($servername, $username, $password,'jetadmin');

![[Pasted image 20240406174604.png]]

i connected to the db with these creds :  jet/dcr46kdl6zsld68idtyufldro

![[Pasted image 20240406174718.png]]

the databases didnt give any information : 
but ifound this 
![[Pasted image 20240406175401.png]]