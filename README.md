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
After reviewed the fsocity.dic file, i could assume that the file maybe containing some combination of passwords.

Then, after launching the directory buster on the target, i found that the website got wordpress login but we were not gonna `bruteforce` method to login the *wp-admin*.

```
gobuster dir -u http://ip_addr -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

![gobuster](https://user-images.githubusercontent.com/87742813/206622457-f9e583ca-582e-41a7-97e0-8f59d48d22a3.png)


But the credential to login the administrator's dashboard was actually in /license dir where you can get the username and password.

          User = elliot
          Pass = ER28-0652
ssasaaaaaaaa


