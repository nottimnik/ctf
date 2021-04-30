# Vulnversity [ [Room](https://tryhackme.com/room/vulnversity) ]

## Task 1

The first didn't required any answers, I just needed to deploy the machine and
connect to the network.

## Task 2

### There are many nmap "cheatsheets" online that you can use too.

Didn't required any answer.

### Scan the box, how many ports are open?

I used the command `sudo nmap [IP_ADDRESS]` to scan for opened ports

Got this report:
```
Nmap scan report for 10.10.5.142
Host is up (0.066s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3128/tcp open  squid-http
3333/tcp open  dec-notes
```

So the number of opened ports is `6`.

### What version of the squid proxy is running on the machine?

I used the command `sudo nmap -sV [ID_ADDRESS]` to determine the version of the
services running

Got this report:
```
Nmap scan report for 10.10.5.142
Host is up (0.086s latency).
Not shown: 994 closed ports
PORT     STATE SERVICE     VERSION
21/tcp   open  ftp         vsftpd 3.0.3
22/tcp   open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.7 (Ubuntu Linux; protocol 2.0)
139/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp  open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
3128/tcp open  http-proxy  Squid http proxy 3.5.12
3333/tcp open  http        Apache httpd 2.4.18 ((Ubuntu))
Service Info: Host: VULNUNIVERSITY; OSs: Unix, Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 31.41 seconds
```

And found out the version of the Squid Proxy:
`3128/tcp open  http-proxy  Squid http proxy 3.5.12`

So, the correct answer is `3.5.12`.

### How many ports will nmap scan if the flag -p-400 was used?

By using `-p-400` nmap will scan the first 400 ports. The answer is `400`.

### Using the nmap flag -n what will it not resolve?

By using `-n` nmap will not perform DNS resolution. The answer is `dns`.

### What is the most likely operating system this machine is running?

I saw that the answer has 6 letters and I guessed that the operating system is
`Ubuntu`.

### What port is the web server running on?

I thought that the port that is running the web server is 3128, because the service
on the ports is `squid-http`:

`3128/tcp open  squid-http`

But the answer is not correct and the only other opened port is `3333`.

The answer is `3333`.

## Task 3

### What is the directory that has an upload form page?

I used `gobuster` to scan the website for URLs and found this urls:

```
gobuster -u http://10.10.5.142:3333 -w /usr/bin/wordlist

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://10.10.5.142:3333/
[+] Threads      : 10
[+] Wordlist     : /usr/bin/wordlist
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2021/04/30 15:18:09 Starting gobuster
=====================================================
/css (Status: 301)
/images (Status: 301)
/internal (Status: 301)
/js (Status: 301)
=====================================================
2021/04/30 15:18:21 Finished
=====================================================
```

The answer has 8 letters and the only url that has 8 letters is `internal`, so the answer is `/internal/`.
