# Bandit - Walkthrough
---
## Connecting
```zsh
$ ssh bandit.labs.overthewire.org -p 2220 -l bandit{number}
[..]
# Paste user password

# If you then want to switch user, use
# Ctrl-d and then reconnect using different login credentials
```

## Levels

**Each level description contains**
+ description
+ commands
+ password

### Level 0

> The goal of this level is for you to log into the game using SSH. The host to which you need to connect is bandit.labs.overthewire.org, on port 2220. The username is bandit0 and the password is bandit0. Once logged in, go to the Level 1 page to find out how to beat Level 1.

```zsh
    # Learn how to use ssh
$ man ssh
    # And then connect to the host
    # like in the section above
```
Login and password are found in level description. Both are `bandit0`

### Level 0 => 1

> The password for the next level is stored in a file called readme located in the home directory. Use this password to log into bandit1 using SSH. Whenever you find a password for a level, use SSH (on port 2220) to log into that level and continue the game.

Flag is in the working directory

```zsh
$ ls # List files
$ cat readme # File's text to stout
```
`boJ9jbbUNNfktd78OOpsqOltutMc3MY1` is the password.

### Level 1 => 2

> The password for the next level is stored in a file called - located in the home directory

Here, the file is a dash (`-`). Do the same thing as before.

```zsh
$ cat - # IT DOESN'T WORK, because '-' is a prefix for options (-d -e -w etc.)

$ cat ./- # It works!
    # './' is the current directory
    # Followed by a file name '-'
```
`CV1DtqXWVFXTvM2F0k09SHz0YwRINYA9` is the password.

### Level 2 => 3

> The password for the next level is stored in a file called spaces in this filename located in the home directory

The same thing, just with spaces (` `) in the filename.

```zsh
    # Hint: use tab autocompletion
$ cat spaces\ in\ this\ filename
# OR
$ cat "spaces in this filename"
```

`UmHadQclWmgdLOKQ3YNgjWxGoRMb5luK` is the password.

### Level 3 => 4

> The password for the next level is stored in a hidden file in the inhere directory.

Folder with hidden file.

```zsh
$ cd inhere/ # Change dir to ~/inhere

$ ls -a # List all files
# And see what's inside
```

`pIwrPrtPN36QITSp3EQaw936yaFoFgAB` is the password.

### Level 4 => 5

> The password for the next level is stored in the only human-readable file in the inhere directory. Tip: if your terminal is messed up, try the “reset” command.

Go into folder, there are many files preceded by a dash (`-`).

ls output:
```zsh
-file00  -file02  -file04  -file06  -file08
-file01  -file03  -file05  -file07  -file09
```

The file command shows file's type, so let's use it
```zsh
$ file ./-file0* # ./, because in the filename there's a dash;
# ('*') at the end allows to select all files, that start with '-file0'
```
Output
```zsh 
./-file00: data
./-file01: data
./-file02: data
./-file03: data
./-file04: data
./-file05: data
./-file06: data
./-file07: ASCII text
./-file08: data
./-file09: data
```
-file07 has a type of ASCII text which is the only human-readable file in here, so check what's inside.

`koReBOKuIDDepwhWk7jZC0RTdopnAYKh` is the flag

### Level 5 => 6

> The password for the next level is stored in a file somewhere under the inhere directory and has all of the following properties:

> - human-readable    
>  - 1033 bytes in size    
>  - not executable  


There's a small problem... namely, multiple folders with files in them. Checking every single one manually would be slow and inefficient, so we will use a couple commands instead.

The *find* command gives us what we need. It can search for files / directories, their size etc. We are looking for file that is 1033 bytes in size, so let's do it, shall we? :)

```zsh
$ find -type f -size 1033c # -type option can be omitted
```
Here, we are looking for a file with size of 1033 bytes. Suffix 'b' stands for 512-byte blocks, so we need to use 'c'.

It turns out, that there's only one file that meets our criteria. Look inside.

`DXjZPULLxYr17uwoI01bNLQbtFemEgo7` is the flag

Check find's man page for more options.

### Level 6 => 7

> The password for the next level is stored somewhere on the server and has all of the following properties:

> - owned by user bandit7
> - owned by group bandit6
> - 33 bytes in size


Again, find will be useful in this case. It can also find files owned by a specific user.

```zsh
$ find / -size 33c -user bandit7 -group bandit6
# Start from root dir ('/'),
# file size 33 bytes,
# owned by user bandit7,
# owned by group bandit6
```
And there's the file **/var/lib/dpkg/info/bandit7.password**.
Use cat on the file.
`HKBPTKQnIay4Fw76bEy8PVxKEDQRKTzs` is the flag.

### Level 7 => 8

> The password for the next level is stored in the file data.txt next to the word millionth

Use grep for matching patterns.
```zsh
$ cat data.txt | grep millionth
```

`cvX2JJa4CFALtqS87jk27qwqGhBM9plV` is the flag.

### Level 8 => 9

> The password for the next level is stored in the file data.txt and is the only line of text that occurs only once

Now the task is to find a line that doesn't repeat. Command 'uniq' does exactly that, but there's one problem. Uniq reports or ommits repeated lines, but needs to have sorted input. Command *sort* speaks for itself. Let's use it.

```zsh
$ cat data.txt | sort | uniq -u
# -u option in uniq displays only lines, that don't repeat, which is what we want
```

`UsvVyFSfZZWbi6wgC7dAFyFuR6jQQUhR` is the flag.

### Level 9 => 10

> The password for the next level is stored in the file data.txt in one of the few human-readable strings, preceded by several ‘=’ characters.

*Strings* command takes input and displays any human-readable strings it found. Let's use it.

```zsh
$ strings data.txt
```
Many results are shown to our eyes. If you scroll up, you can see a couple of results with leading '=' characters. Let's use grep on it.

```zsh
$ strings data.txt | grep ===
```
Command output:

```
========== the*2i"4
========== password
Z)========== is
&========== truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk
```
Now we can see 4 results. The password is `truKLdjsbJ5g7yyJ2X2R0o3a5HQJFuLk`

### Level 10 => 11

> The password for the next level is stored in the file data.txt, which contains [base64](https://en.wikipedia.org/wiki/Base64) encoded data

If we cat data.txt, we can see a string that makes no sense. That's because it was encoded in base64 (click the link to learn more).

The command "base64" is used for... I'll let you guess :). See the man page and use it.

```zsh
$ base64 -d data.txt
```

And voilà. The flag is `IFukwKGsFW8MOq3IRFqrxE1hxTNEbUPR`

### Level 11 => 12

> The password for the next level is stored in the file data.txt, where all lowercase (a-z) and uppercase (A-Z) letters have been rotated by 13 positions

Seems like it's the famous [Rot13](https://en.wikipedia.org/wiki/ROT13) substitution cipher.

I'll use the example from Wikipedia, where the 'tr' command is used.

The *tr* command **t r** -anslates or deletes characters, just like its name suggests.

```zsh
# Map upper case A-Z to N-ZA-M and lower-case a-z to n-za-m
$ echo data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
Remember that this cipher works in both ways, so you can encrypt and decrypt messages using the same command.

The password is `5Te8Y4drgCRfCx8ugdwuEX8KFC6k2EUu`

### Level 12 => 13

[Hex dump](https://en.wikipedia.org/wiki/Hex_dump) on Wikipedia.

> The password for the next level is stored in the file data.txt, which is a hexdump of a file that has been repeatedly compressed. For this level it may be useful to create a directory under /tmp in which you can work using mkdir. For example: mkdir /tmp/myname123. Then copy the datafile using cp, and rename it using mv (read the manpages!)

Let's check which functions are available on this machine.

```zsh
$ which hexdump hd od xxd dump D
/usr/bin/hexdump
/usr/bin/hd
/usr/bin/od
/usr/bin/xxd
```
So there are four of them. I'll use `xxd` as of my personal choice (read the manpage). Create an empty directory under /tmp, so we don't mess with the real file. Then copy the file to that directory and (if you wish) rename it.

Now we can work on it.

```zsh
$ xxd -r data.txt output
```
Using *file* cmd on the file *output*:
```
output: gzip compressed data, was "data2.bin", last modified: Thu May  7 18:14:30 2020, max compression, from Unix
```

The `gzip` program is used to compress data. In contrast, `gunzip` is for expanding data.

```zsh
$ xxd -r data.txt
```
Running file shows, that this is a zip file. Unzip it with (you guessed it) `unzip`.

There are multiple levels of compression, so I won't write down every single command. Use man pages and the internet to help yourself.

I found a shell script that may be handy
```zsh
#!/bin/bash

if [ -f $1 ] ; then
  case $1 in
    *.tar.bz2)  tar xjf $1   ;;
    *.tar.gz)   tar xzf $1   ;;
    *.tar.xz)   tar zxvf $1  ;;
    *.bz2)      bunzip2 $1   ;;
    *.rar)      rar x $1     ;;
    *.gz)       gunzip $1    ;;
    *.tar)      tar xf $1    ;;
    *.tbz2)     tar xjf $1   ;;
    *.tgz)      tar xzf $1   ;;
    *.xz)       xz -d $1     ;;
    *.zip)      unzip $1     ;;
    *.Z)        uncompress $1;;
    *)          echo "contents of '$1' cannot be extracted" ;;
  esac
else
  echo "'$1' is not recognized as a compressed file"
fi
```
I've dug to the end of this crazy compression tunnel manually :(

The password is `8ZjyCRiBWFYkneahHwxCv3wb2a1ORpYL`

### Level 13 => 14

> The password for the next level is stored in /etc/bandit_pass/bandit14 and can only be read by user bandit14. For this level, you don’t get the next password, but you get a private SSH key that can be used to log into the next level. Note: localhost is a hostname that refers to the machine you are working on

In this level, the only thing we get is an ssh password for bandit14. Use ssh.

```zsh
$ ssh -i sshkey.private bandit14@localhost
```
Voilà! If you are curious, the password is inside /etc/bandit_pass/bandit14 file... `4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e`

### Level 14 => 15

> The password for the next level can be retrieved by submitting the password of the current level to port 30000 on localhost.

Use *nc* to send things through TCP/UDP (read the manpage)
```zsh
$ echo 4wcYUJFw0k0XLShlDzztnTBHiqxU3b3e | nc localhost 30000
```

And there we get our password! `BfMYroe26WYalil77FoDi9qh59eK5xNr`

### Level 15 => 16

> The password for the next level can be retrieved by submitting the password of the current level to port 30001 on localhost using SSL encryption.

> Helpful note: Getting “HEARTBEATING” and “Read R BLOCK”? Use -ign_eof and read the “CONNECTED COMMANDS” section in the manpage. Next to ‘R’ and ‘Q’, the ‘B’ command also works in this version of that command…

I'll use openssl s_client for this. First we connect via ssl, and then provide the password.

```zsh
$ openssl s_client -connect localhost:30001


[...]
BfMYroe26WYalil77FoDi9qh59eK5xNr
Correct!
cluFn7wTiGryunymYOu4RcffSxQluehd

closed
```

The password is `cluFn7wTiGryunymYOu4RcffSxQluehd`

### Level 16 => 17

> The credentials for the next level can be retrieved by submitting the password of the current level to a port on localhost in the range 31000 to 32000. First find out which of these ports have a server listening on them. Then find out which of those speak SSL and which don’t. There is only 1 server that will give the next credentials, the others will simply send back to you whatever you send to it.

Get user password from /etc/bandit_pass/bandit16 is necessary

```zsh
# Scan localhost ports
$ nmap -p 31000-32000 localhost
```

It seems like there are only 5 results. And after a bit of trial and error I can confidently say, that the correct one is on port `31790`

```zsh
$ openssl s_client -connect localhost:31790

# Now paste user password
cluFn7wTiGryunymYOu4RcffSxQluehd
# Reply
Correct!

-----BEGIN RSA PRIVATE KEY-----
MIIEogIBAAKCAQEAvmOkuifmMg6HL2YPIOjon6iWfbp7c3jx34YkYWqUH57SUdyJ
imZzeyGC0gtZPGujUSxiJSWI/oTqexh+cAMTSMlOJf7+BrJObArnxd9Y7YT2bRPQ
Ja6Lzb558YW3FZl87ORiO+rW4LCDCNd2lUvLE/GL2GWyuKN0K5iCd5TbtJzEkQTu
DSt2mcNn4rhAL+JFr56o4T6z8WWAW18BR6yGrMq7Q/kALHYW3OekePQAzL0VUYbW
JGTi65CxbCnzc/w4+mqQyvmzpWtMAzJTzAzQxNbkR2MBGySxDLrjg0LWN6sK7wNX
x0YVztz/zbIkPjfkU1jHS+9EbVNj+D1XFOJuaQIDAQABAoIBABagpxpM1aoLWfvD
KHcj10nqcoBc4oE11aFYQwik7xfW+24pRNuDE6SFthOar69jp5RlLwD1NhPx3iBl
J9nOM8OJ0VToum43UOS8YxF8WwhXriYGnc1sskbwpXOUDc9uX4+UESzH22P29ovd
d8WErY0gPxun8pbJLmxkAtWNhpMvfe0050vk9TL5wqbu9AlbssgTcCXkMQnPw9nC
YNN6DDP2lbcBrvgT9YCNL6C+ZKufD52yOQ9qOkwFTEQpjtF4uNtJom+asvlpmS8A
vLY9r60wYSvmZhNqBUrj7lyCtXMIu1kkd4w7F77k+DjHoAXyxcUp1DGL51sOmama
+TOWWgECgYEA8JtPxP0GRJ+IQkX262jM3dEIkza8ky5moIwUqYdsx0NxHgRRhORT
8c8hAuRBb2G82so8vUHk/fur85OEfc9TncnCY2crpoqsghifKLxrLgtT+qDpfZnx
SatLdt8GfQ85yA7hnWWJ2MxF3NaeSDm75Lsm+tBbAiyc9P2jGRNtMSkCgYEAypHd
HCctNi/FwjulhttFx/rHYKhLidZDFYeiE/v45bN4yFm8x7R/b0iE7KaszX+Exdvt
SghaTdcG0Knyw1bpJVyusavPzpaJMjdJ6tcFhVAbAjm7enCIvGCSx+X3l5SiWg0A
R57hJglezIiVjv3aGwHwvlZvtszK6zV6oXFAu0ECgYAbjo46T4hyP5tJi93V5HDi
Ttiek7xRVxUl+iU7rWkGAXFpMLFteQEsRr7PJ/lemmEY5eTDAFMLy9FL2m9oQWCg
R8VdwSk8r9FGLS+9aKcV5PI/WEKlwgXinB3OhYimtiG2Cg5JCqIZFHxD6MjEGOiu
L8ktHMPvodBwNsSBULpG0QKBgBAplTfC1HOnWiMGOU3KPwYWt0O6CdTkmJOmL8Ni
blh9elyZ9FsGxsgtRBXRsqXuz7wtsQAgLHxbdLq/ZJQ7YfzOKU4ZxEnabvXnvWkU
YOdjHdSOoKvDQNWu6ucyLRAWFuISeXw9a/9p7ftpxm0TSgyvmfLF2MIAEwyzRqaM
77pBAoGAMmjmIJdjp+Ez8duyn3ieo36yrttF5NSsJLAbxFpdlc1gvtGCWW+9Cq0b
dxviW8+TFVEBl1O4f7HVm6EpTscdDxU+bCXWkfjuRb7Dy9GOtt9JPsX8MBTakzh3
vBgsyi/sN3RqRBcGU40fOoZyfAMT8s1m/uYv52O6IgeuZ/ujbjY=
-----END RSA PRIVATE KEY-----
```

We got ourselves the private key, let's use it

```zsh
# Put the output into a file
$ mkdir /tmp/myown123
$ touch /tmp/myown123/rsa.key
$ cat > /tmp/myown123/rsa.key
# Here paste the private key

# Change file's permissions (if not, ssh will not use it as it is "too open")
$ chmod 600 /tmp/myown123/rsa.key

# And now we can log in
$ ssh -i /tmp/myown123/rsa.key bandit17@localhost
```

For curious, the password is `xLYVMN9WE5zQ5vHacb0sZEVqbrp7nBTn`

### Level 17 => 18

> There are 2 files in the homedirectory: passwords.old and passwords.new. The password for the next level is in passwords.new and is the only line that has been changed between passwords.old and passwords.new

> **NOTE:** if you have solved this level and see ‘Byebye!’ when trying to log into bandit18, this is related to the next level, bandit19

Looks like a perfect opportunity for `diff` usage. Diff compares files and reports what **d i f f e r s** between them. 

```zsh
$ diff passwords.old passwords.new

# Output
42c42
< w0Yfolrc5bwjS4qw5mq1nnQi6mF03bii
---
> kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd
```
Here we see clearly see that the new password has only one line changed, for which we were looking for.

So, the password is `kfBf3eYk5BPBRzwjqutbbfE887SVc5Yd`

### Level 18 => 19

> The password for the next level is stored in a file readme in the homedirectory. Unfortunately, someone has modified .bashrc to log you out when you log in with SSH.

When trying to log in
```zsh
# After logging in, first thing we see is
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

Someone is obviously pulling our leg...  ;-)

I tried logging back to bandit17, and then going to bandit18's home directory. The file is there, but we have no permission to access it

```zsh
$ ls -l

total 4
-rw-r----- 1 bandit19 bandit18 33 May  7  2020 readme
```

This doesn't work. 
On the other hand, ssh has an option to send commands to remote hosts. Ideal.
```zsh
$ ssh bandit.labs.overthewire.org -p 2220 -l bandit18 "cat ~/readme"
```

And there we have it!  
`IueksS7Ubh8G3DCwVzrTd8rAVOwq3M5x`

### Level 19 => 20

> To gain access to the next level, you should use the setuid binary in the home directory. Execute it without arguments to find out how to use it. The password for this level can be found in the usual place (/etc/bandit_pass), after you have used the setuid binary.

Run *bandit20-do*

```zsh
$ ./bandit20-do
Run a command as another user.
  Example: ./bandit20-do id
```

Check /etc/bandit_pass/bandit20 password as bandit20

```zsh
$ ./bandit20-do cat /etc/bandit_pass/bandit20
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
```

So then `GbKksEFF4yrVs6il55v6gwY5aVje5f0j` is our password.

### Level 20 => 21

> There is a setuid binary in the home directory that does the following: it makes a connection to localhost on the port you specify as a command line argument. It then reads a line of text from the connection and compares it to the password in the previous level (bandit20). If the password is correct, it will transmit the password for the next level (bandit21).

> **NOTE:** Try connecting to your own network daemon to see if it works as you think

Open two terminal sessions (or use tmux, terminator, jobs, etc.)
On the first one, type the following
```zsh
$ nc -lnvp 1234 # nc listen on port 1234 verbose no dns
listening on [any] 1234 ...
```

On the second one, connect using *suconnect*
```zsh
$ ./suconnect 1234
```

Now paste your password to nc and there it is!
Both prompts shown
```zsh
# NC
$ nc -lnvp 1234
listening on [any] 1234 ...
connect to [127.0.0.1] from (UNKNOWN) [127.0.0.1] 44170
GbKksEFF4yrVs6il55v6gwY5aVje5f0j
gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr

# SUCONNECT
$ ./suconnect 1234
Read: GbKksEFF4yrVs6il55v6gwY5aVje5f0j
Password matches, sending next password
```

The password is `gE269g2h3mw3pwgrj0Ha9Uoqen1c9DGr`

### Level 21 => 22

> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

Let's list every file in /etc/cron.d/ directory as suggested
```zsh
$ ls -Al /etc/cron.d
total 28
-rw-r--r-- 1 root root  62 May 14  2020 cronjob_bandit15_root
-rw-r--r-- 1 root root  62 Jul 11  2020 cronjob_bandit17_root
-rw-r--r-- 1 root root 120 May  7  2020 cronjob_bandit22
-rw-r--r-- 1 root root 122 May  7  2020 cronjob_bandit23
-rw-r--r-- 1 root root 120 May 14  2020 cronjob_bandit24
-rw-r--r-- 1 root root  62 May 14  2020 cronjob_bandit25_root
-rw-r--r-- 1 root root 102 Oct  7  2017 .placeholder
```

We can read every one of them. Great! Cat *cronjob_bandit22
```zsh
$ cat /etc/cron.d/cronjob_bandit22
@reboot bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
* * * * * bandit22 /usr/bin/cronjob_bandit22.sh &> /dev/null
```

It's clear that cron runs *cronjob_bandit22.sh* script on reebot and every second. Let's find out what is does.
```zsh
$ cat /usr/bin/cronjob_bandit22.sh
#!/bin/bash
chmod 644 /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
cat /etc/bandit_pass/bandit22 > /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
```

The script appends bandit22's password to */tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv* Cat the file...
```zsh
$ cat /tmp/t7O6lds9S0RqQh9aMcz6ShpAoZKF7fgv
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```
...and we get the password `Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI`

### Level 22 => 23

> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

> **NOTE:** Looking at shell scripts written by other people is a very useful skill. The script for this level is intentionally made easy to read. If you are having problems understanding what it does, try executing it to see the debug information it prints.

See what is inside */etc/cron.d/cronjob_bandit23*
```zsh
$ cat /etc/cron.d/cronjob_bandit23
@reboot bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
* * * * * bandit23 /usr/bin/cronjob_bandit23.sh  &> /dev/null
```

Check /usr/bin/cronjob_bandit23.sh
```zsh
cat /usr/bin/cronjob_bandit23.sh
#!/bin/bash

myname=$(whoami)
mytarget=$(echo I am user $myname | md5sum | cut -d ' ' -f 1)

echo "Copying passwordfile /etc/bandit_pass/$myname to /tmp/$mytarget"

cat /etc/bandit_pass/$myname > /tmp/$mytarget
```
It creates a file and appends password to it.
Run the script and *cat* it's output file.
```zsh
$ bash /usr/bin/cronjob_bandit23.sh
Copying passwordfile /etc/bandit_pass/bandit22 to /tmp/8169b67bd894ddbb4412f91573b38db3

$ cat /tmp/8169b67bd894ddbb4412f91573b38db3
Yk7owGAcWjwMVRwrTesJEwB7WVOiILLI
```
But it's **not** bandit23's password. It is bandit's 22 password.
Paste ` echo I am user $myname | md5sum | cut -d ' ' -f 1` from the script to command line. Change *$myname* to *bandit23*. Now press ENTER
```zsh
$ echo I am user bandit23 | md5sum | cut -d ' ' -f 1
8ca319486bfbbc3663ea0fbe81326349
```
The output is the file, which we are looking for. It's located under */tmp*
```zsh
$ cat /tmp/8ca319486bfbbc3663ea0fbe81326349
jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n
```
Bandit23's password is `jc1udXuA1tiHqjIsL8yaapX5XIAI6i0n`

### Level 23 => 24

> A program is running automatically at regular intervals from cron, the time-based job scheduler. Look in /etc/cron.d/ for the configuration and see what command is being executed.

> **NOTE:** This level requires you to create your own first shell-script. This is a very big step and you should be proud of yourself when you beat this level!

> **NOTE 2:** Keep in mind that your shell script is removed once executed, so you may want to keep a copy around…

cat */etc/cron.d/cronjob_bandit24*
```zsh
$ cat /etc/cron.d/cronjob_bandit24
@reboot bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
* * * * * bandit24 /usr/bin/cronjob_bandit24.sh &> /dev/null
```

Check /usr/bin/cronjob_bandit23.sh
```zsh
$ cat /usr/bin/cronjob_bandit23.sh

#!/bin/bash

myname=$(whoami)

cd /var/spool/$myname
echo "Executing and deleting all scripts in /var/spool/$myname:"
for i in * .*;
do
    if [ "$i" != "." -a "$i" != ".." ];
    then
        echo "Handling $i"
        owner="$(stat --format "%U" ./$i)"
        if [ "${owner}" = "bandit23" ]; then
            timeout -s 9 60 ./$i
        fi
        rm -f ./$i
    fi
done
```

This script executes every file that is in /var/spool/bandit24 (because he's executing cron job). Maybe we can create something inside that directory?

```zsh
$ cd /var/spool/bandit24
bandit23@bandit:/var/spool/bandit24$
```

Well, we can cd into it, but can we create any files? It's possible to check by trial-and-error, but inspecting directory permissions is both faster and easier to check.

```zsh
$ ls -l  /var/spool
total 12
drwxrwx-wx 88 root bandit24 4096 Feb 18 19:49 bandit24
```

Bandit23 has no read permission, but do have write and execute permissions. So we can create files, go inside that directory, but it's neither possible to read nor list the files in that directory.

I created a script in that directory.
```zsh
$ touch getpasswd.sh
$ vi getpasswd.sh

#!/bin/bash

cat /etc/bandit_pass/bandit24 > /tmp/adam23/file.txt
```

This script does a cat on bandit24's password and redirects stdout to /tmp/adam23/file.txt. Now in order for it to execute, it needs the **x** permission (or **1** in octal).

```zsh
$ chmod 777 getpasswd.sh
```

Additionally, the folder must be accessible to every user.

```zsh
$ chmod 777 /tmp/adam23
```

Check what's the time
```zsh
$ date
Fri Feb 18 19:48:49 CET 2022
```

Cron job executes every minute, so wait 'till next minute.

```zsh
$ date
Fri Feb 18 19:49:14 CET 2022
```

Now check what's inside /tmp/adam23/file.txt

Password is `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ`

### Level 24 => 25

> A daemon is listening on port 30002 and will give you the password for bandit25 if given the password for bandit24 and a secret numeric 4-digit pin code. There is no way to retrieve the pin code except by going through all of the 10000 combinations, called brute-forcing.

Well, preforming a *brace expansion* will definitely help.

First, create correct brute-force input
Save to file or display it in terminal

```zsh
echo -e UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ\ {0..9}{0..9}{0..9}{0..9}\ '\n'
```

Here, *echo*'s `-e` option enables patterns such as `\n` (new line). First, echo bandit24's password, then `\[space]` creates a space - it's not a separator. Next, there are four brace expansions going from 0 to 9. At the end is a newline character.

Quick look at the output
```
[...]
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9992
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9993
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9994
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9995
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9996
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9997
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9998
 UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 9999
```

With input ready, use `nc localhost 30002` (pipe) to connect with port 30002.

```zsh
echo -e UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ\ {0..9}{0..9}{0..9}{0..9}\ '\n' | nc localhost 30002

# OUTPUT
I am the pincode checker for user bandit25. Please enter the password for user bandit24 and the secret pincode on a single line, separated by a space.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
[...]
Wrong! Please enter the correct pincode. Try again.
Wrong! Please enter the correct pincode. Try again.
Correct!
The password of user bandit25 is uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG

Exiting.
```

Correct credentials are `UoMYTrfrBFHyQXmg6gzctqAwOmw1IohZ 2588`

The password of user bandit25 is `uNG9O58gUE7snukf3bvZ0rxhtnjzSGzG`

### Level 25 => 26

> Logging in to bandit26 from bandit25 should be fairly easy… The shell for user bandit26 is not /bin/bash, but something else. Find out what it is, how it works and how to break out of it.

```zsh
$ cat /etc/passwd | grep bandit26
bandit26:x:11026:11026:bandit level 26:/home/bandit26:/usr/bin/showtext
```


```zsh
$ cat /usr/bin/showtext
#!/bin/sh

export TERM=linux

more ~/text.txt
exit 0
```

Check **more**'s manpage
```
v  -> Start  up  an  editor  at  current line.  The editor is taken from the environment variable VISUAL if defined, or EDITOR  if  VISUAL  is  not defined, or defaults to vi if neither VISUAL nor EDITOR is defined.
```

Shrink your terminal, so that is smaller than ~5 lines (less than *file.txt* output). Then log in via ssh. Next, press `v`, which will change editor to vi. Next up, type the following

```vi
:set shell=/bin/bash
:shell
```

And we are back in bash. *cat* bandit26 password.

The password is `5czgV9L3Xx8JPOyRbXh6lQbmIOWvPT6Z`

### Level 26 => 27

> Good job getting a shell! Now hurry and grab the password for bandit27!

Nothing hard, just ls and use that script.

```zsh
$ ls -Ahl
total 28K
-rwsr-x--- 1 bandit27 bandit26 7.2K May  7  2020 bandit27-do # it was in red

$ ./bandit27-do
Run a command as another user.
  Example: ./bandit27-do id

$ ./bandit27-do cat /etc/bandit_pass/bandit27
3ba3118a22e93127a4ed485be72ef5ea
```

The password is `3ba3118a22e93127a4ed485be72ef5ea`

### Level 27 => 28

> There is a git repository at ssh://bandit27-git@localhost/home/bandit27-git/repo. The password for the user bandit27-git is the same as for the user bandit27.

Create tmp directory. Clone repo to that directory and look inside.

```zsh
$ git clone ssh://bandit27-git@localhost/home/bandit27-git/repo
Cloning into 'repo'...
Could not create directory '/home/bandit27/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit27/.ssh/known_hosts).
This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit27-git@localhost's password:
remote: Counting objects: 3, done.
remote: Compressing objects: 100% (2/2), done.
remote: Total 3 (delta 0), reused 0 (delta 0)
Receiving objects: 100% (3/3), done.

$ ls
passwd.txt  repo

$ cd repo/
bandit27@bandit:/tmp/adam12/repo$ ls
README

$ cat README
```

The password to the next level is: `0ef186ac70e04ea33b4c1853d2526fa2`

### Level 28 => 29

> There is a git repository at ssh://bandit28-git@localhost/home/bandit28-git/repo. The password for the user bandit28-git is the same as for the user bandit28.

Same thing as before.

```zsh
$ git clone ssh://bandit28-git@localhost/home/bandit28-git/repo
# Provide bandit28 password

$ cd repo/
bandit27@bandit:/tmp/adam12/repo$ ls
README.md
bandit27@bandit:/tmp/adam12/repo$ cat README.md
# Bandit Notes
Some notes for level29 of bandit.

## credentials

- username: bandit29
- password: xxxxxxxxxx
```

Check git log
```zsh 
$ git log
commit edd935d60906b33f0619605abd1689808ccdd5ee
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    fix info leak

commit c086d11a00c0648d095d04c089786efef5e01264
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    add missing data

commit de2ebe2d5fd1598cd547f4d56247e053be3fdc38
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:49 2020 +0200

    initial commit of README.md
```

Aha! It was changed. Inspect changes by running *git diff*

```zsh
$ git diff c086d11a00c0648d095d04c089786efef5e01264 de2ebe2d5fd1598cd547f4d56247e053be3fdc38
diff --git a/README.md b/README.md
index 3f7cee8..7ba2d2f 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for level29 of bandit.
 ## credentials

 - username: bandit29
-- password: bbc96594b4e001778eee9975372716b2
+- password: <TBD>
```

The password is `bbc96594b4e001778eee9975372716b2`

### Level 29 => 30

> There is a git repository at ssh://bandit29-git@localhost/home/bandit29-git/repo. The password for the user bandit29-git is the same as for the user bandit29.

It's very similar to the previous level in a sense that we must first clone the repo into the tmp directory. 

```zsh
/tmp/adam23$ git clone ssh://bandit29-git@localhost/home/bandit29-git/repo

# Next, provide bandit29's password
```

Now *cd* and look inside.

There is only one file called README.md.
```zsh
$ cat README.md
# Bandit Notes
Some notes for bandit30 of bandit.

## credentials

- username: bandit30
- password: <no passwords in production!>
```

No password in production... what about the diff file?  
*The -p flag is for displaying diff files*

```zsh
$ git log -p 
commit 208f463b5b3992906eabf23c562eda3277fea912
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:51 2020 +0200

    fix username

diff --git a/README.md b/README.md
index 2da2f39..1af21d3 100644
--- a/README.md
+++ b/README.md
@@ -3,6 +3,6 @@ Some notes for bandit30 of bandit.

 ## credentials

-- username: bandit29
+- username: bandit30
 - password: <no passwords in production!>


commit 18a6fd6d5ef7f0874bbdda2fa0d77b3b81fd63f7
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:51 2020 +0200

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..2da2f39
--- /dev/null
+++ b/README.md
@@ -0,0 +1,8 @@
+# Bandit Notes
+Some notes for bandit30 of bandit.
+
+## credentials
+
+- username: bandit29
+- password: <no passwords in production!>
```

The same result... What about the other branches?  
*The -r switch shows remote branches*

```zsh
$ git branch -r
  origin/HEAD -> origin/master
  origin/dev
  origin/master
  origin/sploits-dev
```

The *dev* branch looks a bit sus... :D
```zsh
$ git checkout dev
Branch dev set up to track remote branch dev from origin.
Switched to a new branch 'dev'

$ git branch
* dev
  master
```

Again, *git log -p*
```zsh
$ git log -p
commit bc833286fca18a3948aec989f7025e23ffc16c07
Author: Morla Porla <morla@overthewire.org>
Date:   Thu May 7 20:14:52 2020 +0200

    add data needed for development

diff --git a/README.md b/README.md
index 1af21d3..39b87a8 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit.
index 1af21d3..39b87a8 100644
--- a/README.md
+++ b/README.md
@@ -4,5 +4,5 @@ Some notes for bandit30 of bandit.
 ## credentials

 - username: bandit30
-- password: <no passwords in production!>
+- password: 5b90576bedb2cc04c86a9e924ce42faf


commit 8e6c203f885bd4cd77602f8b9a9ea479929ffa57
```

Aha! 

Password: `5b90576bedb2cc04c86a9e924ce42faf`

### Level 30 => 31

> There is a git repository at ssh://bandit30-git@localhost/home/bandit30-git/repo. The password for the user bandit30-git is the same as for the user bandit30.

Well, well. We meet yet again!
- Clone git repo (put a directory path at the end if you like)
- cd in
- See what's inside
- We don't speak about the git-clone ever again

```zsh
$ git log -p
commit 3aefa229469b7ba1cc08203e5d8fa299354c496b
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:54 2020 +0200

    initial commit of README.md

diff --git a/README.md b/README.md
new file mode 100644
index 0000000..029ba42
--- /dev/null
+++ b/README.md
@@ -0,0 +1 @@
+just an epmty file... muahaha
```

Seems like someone has a sense of humor... not for long.

Behold! Use *git branch -r* just like before

```zsh
$ git branch -r
  origin/HEAD -> origin/master
  origin/master
```

Defeat? No! Search deeper!

```zsh
$ git tag
  secret

# Muhahahah

$ git show secret
  47e603bb428404d265f59c42920d81e5
```

Ladies and gentlemen, we got'em.

The pass word is `47e603bb428404d265f59c42920d81e5`

### Level 31 => 32

> There is a git repository at ssh://bandit31-git@localhost/home/bandit31-git/repo. The password for the user bandit31-git is the same as for the user bandit31.

Here we go again.

```zsh
$ ls -Ah
.git  .gitignore  README.md
bandit31@bandit:/tmp/adam23/ripo$ git log -p
commit 701b33b545902a670a46088731949ae040983d80
Author: Ben Dover <noone@overthewire.org>
Date:   Thu May 7 20:14:56 2020 +0200

    initial commit

diff --git a/.gitignore b/.gitignore
new file mode 100644
index 0000000..2211df6
--- /dev/null
+++ b/.gitignore
@@ -0,0 +1 @@
+*.txt
diff --git a/README.md b/README.md
new file mode 100644
index 0000000..0edecc0
--- /dev/null
+++ b/README.md
@@ -0,0 +1,7 @@
+This time your task is to push a file to the remote repository.
+
+Details:
+    File name: key.txt
+    Content: 'May I come in?'
+    Branch: master
+
```

This time it's commit-time!

```zsh
bandit31@bandit:/tmp/adam23/ripo$ cat > key.txt
May I come in?
bandit31@bandit:/tmp/adam23/ripo$ cat key.txt

May I come in?
bandit31@bandit:/tmp/adam23/ripo$ git branch
* master

$ vi .gitignore
# Add line '!key.txt' or just remove the .gitignore file

$ git add .
$ git commit -m 'YES'
[master 9ff4e54] YES
 2 files changed, 2 insertions(+)
 create mode 100644 key.txt

$ git push -u origin
Could not create directory '/home/bandit31/.ssh'.
The authenticity of host 'localhost (127.0.0.1)' can't be established.
ECDSA key fingerprint is SHA256:98UL0ZWr85496EtCRkKlo20X3OPnyPSB5tB5RPbhczc.
Are you sure you want to continue connecting (yes/no)? yes
Failed to add the host to the list of known hosts (/home/bandit31/.ssh/known_hosts).This is a OverTheWire game server. More information on http://www.overthewire.org/wargames

bandit31-git@localhost's password:
Counting objects: 4, done.
Delta compression using up to 2 threads.
Compressing objects: 100% (2/2), done.
Writing objects: 100% (4/4), 339 bytes | 0 bytes/s, done.
Total 4 (delta 0), reused 0 (delta 0)
remote: ### Attempting to validate files... ####
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
remote: Well done! Here is the password for the next level:
remote: 56a9bf19c63d650ce78e6ec0354ee45e
remote:
remote: .oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.oOo.
remote:
To ssh://localhost/home/bandit31-git/repo
 ! [remote rejected] master -> master (pre-receive hook declined)
error: failed to push some refs to 'ssh://bandit31-git@localhost/home/bandit31-git/repo'
```

Yay! The password is `56a9bf19c63d650ce78e6ec0354ee45e`

### Level 32 => 33

> After all this git stuff it's time for another escape. Good luck!

```zsh
WELCOME TO THE UPPERCASE SHELL
>> ls
sh: 1: LS: not found
>>
```

Shell converts every letter to uppercase... It needs to be changed. Thankfully, `$0` command allows us to do so.

```zsh
>> $0
$ ls
uppershell
$ pwd
/home/bandit32
$ cat /etc/bandit_pass/bandit33
c9c3199ddf4121b10cf581a98d51caee
```

The password is `c9c3199ddf4121b10cf581a98d51caee`

### Level 33 => 34

What now?

```zsh
$ ls -Ah
.bash_logout  .bashrc  .profile  README.txt
bandit33@bandit:~$ cat README.txt
Congratulations on solving the last level of this game!

At this moment, there are no more levels to play in this game. However, we are constantly working
on new levels and will most likely expand this game with more levels soon.
Keep an eye out for an announcement on our usual communication channels!
In the meantime, you could play some of our other wargames.

If you have an idea for an awesome new level, please let us know!
```
**And so, it is done!**

# Personal opinion

I appreciate every bit of work that was done in order to create such a fantastic game. It's not perfect - like everything in our world - but I was having fun playing it, which is important.