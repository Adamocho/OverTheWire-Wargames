# Leviathan - Walkthrough
---
## Connecting
```zsh
$ ssh leviathan.labs.overthewire.org -p 2223 -l leviathan{number}
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

```zsh
$ ./check
    password: rioGegei8m
    Wrong password, Good Bye ...
```

It looks like it compares strings. Run `ltrace` on it (see the manpage).

```zsh
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

Will ltrace unveil the mistery?

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
It does it by looking at the **OWNER** of the file (real user), not at the executor (effective user). That's it! All there's left to do is to create a file that referes to */etc/leviathan_pass/leviathan3*.

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

We're in. Let me guide you throgh what just happened:
    - script accepts correct password
    - calls *geteuid()* twice. That means it will receive 12004 uid. In this case it's *12002* simply because we are using ltrace on it.
    - sets user id to that number
    - loggs into bash

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

It truns out that `Tith4cokei` is the password.