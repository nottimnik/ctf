# Look-For-It (30pts)

## Description

Look for `flag.txt` **Note:** This is a very easy 30 point challenge and does not require the use of enumeration tools.

## Attachments

http://lookforit.epizy.com/

Category: Web
Author: ainz

# Write-Up

First, I scanned the domain:
`nmap -n http://lookforit.epizy.com/`

```
Starting Nmap 7.80 ( https://nmap.org ) at 2021-04-30 18:52 EDT
Nmap scan report for lookforit.epizy.com (185.27.134.112)
Host is up (0.060s latency).
Not shown: 991 closed ports
PORT     STATE    SERVICE
19/tcp   filtered chargen
25/tcp   filtered smtp
80/tcp   open     http
111/tcp  open     rpcbind
135/tcp  filtered msrpc
139/tcp  filtered netbios-ssn
443/tcp  open     https
445/tcp  filtered microsoft-ds
2049/tcp open     nfs
```

Found a lot of open ports, but I tried focus on the note that the challenge is easy and can be done without any tools.

So, I looked at the source code of the pages and didn't find anything important.

Then I thought that there might be some secret urls, so I tried to bruteforce the urls using `gobuster`:

```
gobuster -u http://lookforit.epizy.com/ -w /usr/bin/wordlist

=====================================================
Gobuster v2.0.1              OJ Reeves (@TheColonial)
=====================================================
[+] Mode         : dir
[+] Url/Domain   : http://lookforit.epizy.com/
[+] Threads      : 10
[+] Wordlist     : /usr/bin/wordlist
[+] Status codes : 200,204,301,302,307,403
[+] Timeout      : 10s
=====================================================
2021/04/30 18:59:15 Starting gobuster
=====================================================
2021/04/30 18:59:15 [-] Wildcard response found: http://lookforit.epizy.com/a02388d5-e0c5-4ccf-87fd-d874d70d2362 => 200
2021/04/30 18:59:15 [!] To force processing of Wildcard responses, specify the '-fw' switch.
=====================================================
2021/04/30 18:59:15 Finished
=====================================================
```

But didn't find anything, probably because the site is using `php`.
