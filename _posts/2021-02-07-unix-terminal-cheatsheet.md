---
layout: post
title:  "Unix Cheatsheet"
date:   2021-02-07 12:00:00 +0100
categories: unix
tags: cheatsheet terminal
---
<style>
:root {
--theme-body-font-family:Arial,"Helvetica Neue",Helvetica,sans-serif;
--black-800:#242729;
--white:#fff;
--black-075:#e4e6e8;
--black-300:#9fa6ad;
}
kbd {
    display: inline-block;
    margin: 0 .1em;
    padding: .1em .6em;
    font-family: var(--theme-body-font-family);
    font-size: 11px;
    line-height: 1.4;
    color: var(--black-800);
    text-shadow: 0 1px 0 var(--white);
    background-color: var(--black-075);
    border: 1px solid var(--black-300);
    border-radius: 3px;
    box-shadow: 0 1px 1px rgba(12,13,14,0.15),inset 0 1px 0 0 var(--white);
    white-space: nowrap;
}
</style>
<!--more-->
Legend for some commands (do not include the brackets) : [optional field], &lt;mandatory field&gt; #comments.
The dollar sign indicates that you are not running the command as 'root' user (user that can change "kernel" components i.e. brightness, firewall, hardware stuff...).

Source: [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/2020/course-shell/)

* Print argument on screen
```console
$ echo hi
hi
```

* Abort command with <kbd>Ctrl</kbd> + <kbd>C</kbd>
```console
$ echo hi^C
```

* Clear the terminal with <kbd>Ctrl</kbd> + <kbd>L</kbd>
* Navigate through previous commands with <kbd>↑</kbd> and <kbd>↓</kbd>
* Move command line pointer to the the start with <kbd>Ctrl</kbd> + <kbd>A</kbd>
* Move command line pointer to the the end with <kbd>Ctrl</kbd> + <kbd>E</kbd>
  * Does not work in VSC terminal

* Show date
```console
$ date
Sat Feb 13 17:53:39 CET 2021
```

* Time (stopwatch) a program
```console
$ time echo hi
hi
real    0m0.000s
user    0m0.000s
sys     0m0.000s
```

* Print current ("working") directory
```console
$ pwd
/mnt/c/Users/sergio/OneDrive/Desktop/skirienkopanea.github.io
```

* Print $PATH variable (which contains directories where shell checks for programs)
```console
$ echo $PATH #among other paths in a : separated list.
/usr/bin/ 
```
A path that starts with / is called an absolute path. Any other path is a relative path. Relative paths are relative to the current working directory, which we can see with the pwd command and change with the cd command. In a path, . refers to the current directory, and .. to its parent directory.

* Print path of the program
```console
$ which echo
/usr/bin/echo
```

* Show manual of the program
```console
$ man <program>
```

* Show program help
```console
$ <program> --help #it's up to the program to provide it though.
```

* List all files and folders in a directory. -a for hidden and -l for more details
```console
$ ls [path] #if no path then it uses the working directory
$ ls /usr/bin/
 resizecons       systemd-escape         appres           ec2metadata
 resizepart       systemd-hwdb           apropos          echo
```

* Change directory
```console
$ cd [folder] #No folder sends you to home (~) directory
```

* Go to previous directory
```console
$ cd -
```

* Rename/move file
```console
$ mv <filepath old> <filepath new>
```

* Copy file
```console
$ cp <filepath origin> <filepath destination>
```

* Remove file
```console
$ rm <filepath>
```

* Remove directory (non-recursive)
```console
$ # $ rm test
$ # rm: cannot remove 'test': Is a directory
$ rmdir test
rmdir: failed to remove 'test': Directory not empty
```

* Remove directory (recursive)
```console
$ rm <directory path> -r
```

* Make new directory
```console
$ mkdir <directory path>
```

* Direct program output to a file with <kbd>&gt;</kbd>
```console
$ man ls > ls_documentation.txt
```

* Print contents of a file in the terminal
```console
$ cat <filepath>
```

* Define program input from a file with <kbd>&lt;</kbd>, and print it on the terimnal
```console
$ cat < ls_documentation.txt
NAME
       ls - list directory contents
SYNOPSIS
       ls [OPTION]... [FILE]...
```

* Combination of <kbd>&lt;</kbd> and <kbd>&gt;</kbd>
```console
$ cat < input_file.txt > output_file.txt
$ cat < input_file.txt > output_file.txt #overwrites
$ cat < input_file.txt > output_file.txt #overwrites
$ cat < output_file.txt #print output on the terimanl
hello there
```
* Append with <kbd>&gt;&gt;</kbd>
```console
$ echo abc >> output_file.txt
$ echo abc >> output_file.txt
$ echo abc >> output_file.txt
$ cat < output_file.txt #print output on the terimanl
hello thereabc
abc
abc
```
* Print last -n&lt;k&gt; lines of its input (by default a file declared in the first paramater)
```console
$ tail -n1 output_file.txt
abc
```
* Print first -n&lt;k&gt; lines of its input (by default a file declared in the first paramater)
```console
$ head -n1 output_file.txt
hello thereabc
```

* Pipe character <kbd>|</kbd> takes left program output as input for right program 
```console
$ ls --help | tail -n2 > output_file.txt
$ cat output_file.txt
Full documentation at: <https://www.gnu.org/software/coreutils/ls>
or available locally via: info '(coreutils) ls invocation'
```

* Execute command as superuser
```console
$ sudo <command>
```
* Open a shell terminal as root user
```console
$ sudo su
[sudo] password for sergio:
#
```
* Exit
```console
exit
```
* Store input to a file but also keep it for other uses (by default print it on the terminal)
```console
$ echo hi | tee log.txt
hi
```
* Search for files and directories
```console
$ find -name '*name*'
```
* Open file in default program
```console
$ xdg-open <filepath>
```

* Update last modified time / create new file
```console
$ touch <filepath>
```
