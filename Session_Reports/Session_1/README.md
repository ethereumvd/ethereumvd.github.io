# Session-1

## Date and venue of the session :
    Conducted on :- Sunday , 11th August 2024
    Venue :- Cyberlabs Room , SAC

## Agenda of the session :
    To learn basic Penetration Testing methods through completion of ROOTME practice box on tryhackme.

## Session report :
### Connecting to the tryhackme network
OpenVPN was installed . The configuration file was downloaded from the website and a connection was established  . 
### Information Gathering 
We were provided with the ip address of the target machine `10.10.144.45`. The first step was to scan for any open ports on the machine using  `nmap -Sv -vvv 10.10.144.45` where port `22` and `80` were found open which were the SSH and http services respectively and the version of Apache running on the server was noted . Now GoBuster and seclists were installed and all the directories on the web server were discoverd using the command `gobuster dir --url 10.10.144.45 --wordlist /usr/share/wordlists/dirbuster/directory-list-1.0.txt`  . Two directories `/panel` and `/uploads` were found . 
### The Exploit 
#### PHP Code Execution and getting reverse shell :
In the `/panel` directory , we had to upload a PHP code to get the reverse shell of the target machine . The website checked for `.php` extension and did not allow it , so after some hit and trials  `.php5` and `.phtml` extensions were found to be allowed . A PHP script to get the reverse shell was used from the pentestmonkey/php-reverse-shell github repo changing the $ip field to our system's ip address and leaving the port as default i.e  `1234` . Now we started a netcat listener on port 1234 using the command `nc -lnvp 1234` and the reverse shell was obtained .
#### Linux Privilege Escalation :
First we had to find user.txt which we did using `find / -type f name user.txt 2>/dev/null` . Reading the file we got the flag `THM{y0u_g0t_a_sh3ll}` . For Privilege Escalation , we needed to fing all files with a SUID permission that can be run as root . This was suspected because user.txt had a SUID bit . Now using `find / -user root -perm /4000`  we listed all files that had SUID permission to be run as root. It was seen that we could run `/usr/bin/python` as root . In order to find an exploit , we went to the GTFObins website to get the following exploit - `python -c 'import os; os.execl("/bin/sh", "sh", "-p")'` and we got the root shell . To find the root.txt file we used the command `find / -type f -name root.txt` and on reading the file , we got the flag `THM{pr1v1l3g3_3sc4l4t10n}` . 


## Resources :
- [ROOTME room (tryhackme)](https://tryhackme.com/r/room/rrootme)
- [NMAP manpage](https://nmap.org/book/man.html)
- [GoBuster manpage](https://linuxcommandlibrary.com/man/gobuster)
- [Bypassing file extension checks (Hacktricks)](https://book.hacktricks.xyz/pentesting-web/file-upload#file-upload-general-methodology)
- [PHP Reverse-shell code (pentestmonkey)](https://github.com/pentestmonkey/php-reverse-shell/blob/master/php-reverse-shell.php)
- [Find command manpage](https://www.man7.org/linux/man-pages/man1/find.1.html)
- [SUID reference](https://en.wikipedia.org/wiki/Setuid)
- [SUID python Exploit (GTFOBins)](https://gtfobins.github.io/gtfobins/python/#suid)

## Summary :
- Discussion of basic Penetration Testing methods 
- Comlpeting ROOTME practice box on tryhackme
