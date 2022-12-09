# mr.robot-THM
Simple box writeup/walkthrough [Mr.robot-THM](https://tryhackme.com/room/mrrobot)
Level : medium

---

## walkthrough

![MRROBOT](https://user-images.githubusercontent.com/87742813/205921850-14ba38d4-e7a1-4e94-b2d0-1a1626de9d57.jpeg)

### Enumeration

By running `nmap -sC -v -sV ip_addr`, we will get the following input.

![mr robot nmap](https://user-images.githubusercontent.com/87742813/205923416-e6307596-8e23-4965-adbf-a7a39d45cb0f.png)

Based on the output given, we noticed that the ip leaves us with port 80 and 443 which can redirect us to the hosting page. Lets go to the webpage as below:

![robot webpage](https://user-images.githubusercontent.com/87742813/205924859-cc45df44-369f-4db0-85bd-5bfabb5e220f.png)

As usual, we cant ignore to visit `robots.txt` if we dealing with CTF because it will give us some hints to further's enumeration.

After redirected to http://ip_addr/robots.txt , we found our first key/flag and hint.

```
user-agent: *
fsocity.dic
key-1-of-3.txt
```
          1st key = 073403c8a58a1f80d943455fb30724b9

After reviewed the fsocity.dic file, i could assume that the file maybe containing some combination of passwords.

Then, after launching the directory buster on the target, i found that the website got wordpress login but we were not gonna `bruteforce` method to login the *wp-admin*.

```
gobuster dir -u http://ip_addr -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

![gobuster](https://user-images.githubusercontent.com/87742813/206622457-f9e583ca-582e-41a7-97e0-8f59d48d22a3.png)


But the credential to login the administrator's dashboard was actually in /license dir where you can get the username and password.

          User = elliot
          Pass = ER28-0652

### Initial Foothold

Here, we have an administrator user. From this point on, we may upload the php-reverse-shell theme for WordPress using the full authority of the administrator account. Select the Editor option under the Appearance tab(404.php).

![themes robot](https://user-images.githubusercontent.com/87742813/206655031-9c29fb21-6584-4abe-89f1-c7b432beecae.png)

Then, we just replace the default php code to reverseshell.php that u can search on [pentest-monkey](https://www.revshells.com/). Do not forget to enter your localhost's ip and select a random port.

After making the necessary template updates, launch `NetCat` and enter the URL's path to the 404 template.

By using the command `rlwrap nc -lnvp your_port`

![nc robot](https://user-images.githubusercontent.com/87742813/206664734-81404a54-f417-4084-96ec-e891f732cd4a.png)

At this point, I used to investigate the system and entered the robot directory, I found two files. The first is our second key/flag, and the second is a password that is saved in MD5 format (read permission is set to everyone). 

![hash rbot](https://user-images.githubusercontent.com/87742813/206668022-5e09f024-85f6-408d-93e7-1443fe0869eb.png)

In order to crack the password, we can use a tools like `johntheripper` but the other way is an online tool called [CrackStation](https://crackstation.net).

![crackrobot](https://user-images.githubusercontent.com/87742813/206669195-b30574b2-6ee6-419e-bece-8a25415e8636.png)

We got the password .!

"abcdefghijklmnopqrstuvwxyz"

Login as the robot user and `cat` the second key/flag. (make sure to spawn pty `python3 -c 'import pty; pty.spawn("/bin/bash")'`)

          2nd key = 822c73956184f694993bede3eb39f959

### R00T
 As the robot user, we can use Linpeas or Lineum for further privelege escalation to gain root access but here i will use the common command to search for all files having SUID bit set which is
          
          find / -perm -u=s -type f 2>/dev/null
 

![suid robot](https://user-images.githubusercontent.com/87742813/206673510-33b32632-5a4b-43d2-b54d-d2944abec6ba.png)







