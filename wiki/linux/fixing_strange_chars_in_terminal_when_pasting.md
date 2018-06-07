# Fixing strange chars in terminal when pasting

Vía:  [https://bbs.archlinux.org/viewtopic.php?pid=1416923#p1416923](https://bbs.archlinux.org/viewtopic.php?pid=1416923#p1416923)

* Sometimes when you paste text in the terminal strange chars appear before and after the text, like this:

Original text in clipboard:

```
 TEXT
```

Pasted text:

```
 0~TEXT~
```

This is because for some reason bracketed paste mode has been activated in that terminal.
To solve it, you simply have to disable it using:

```
 printf "\e[?2004l"
```
