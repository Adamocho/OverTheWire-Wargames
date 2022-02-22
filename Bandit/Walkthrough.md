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

> After all this git stuff its time for another escape. Good luck!

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
** And so it is done! **
# Personal opinion
I appreciate every bit of work that was done in order to create such a fantastic game. It's not perfect - like everything in our world - but I was having fun playing and this is the most important thing!