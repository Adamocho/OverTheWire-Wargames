# Leviathan - Walkthrough
---
## Connecting
```zsh
$ ssh leviathan.labs.overthewire.org -p 2223 -l leviathan{number}
[..]
# Paste user password

# If you then want to switch user, use
# Ctrl-d and then reconect using different login credentials
```

## Levels

**Each level description contains**
+ description
+ commands
+ password

### Level 0

Log in using given cridentials:
- login - leviathan0
- password - leviathan0

### Level 0 => 1

In th home directory there is a hidden *.backup* folder. Inside, a HTML file is located. Cat-ing the file gives a lot of output...

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