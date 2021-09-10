---
layout: post
title:  "Unix: Background and Cheatsheet"
date:   2021-02-07 12:00:00 +0100
categories: unix
tags: cheatsheet
---
<!--more-->



# Table of Contents
- [Table of Contents](#table-of-contents)
  - [Background](#background)
  - [Filesystem](#filesystem)
    - [Permissions](#permissions)
    - [File commands](#file-commands)
  - [Pipes \|](#pipes-)
  - [Text file processing commands](#text-file-processing-commands)
    - [Sorting data](#sorting-data)
    - [Joining data](#joining-data)
  - [Process management](#process-management)
  - [Documentation](#documentation)
  - [Running a command per input line](#running-a-command-per-input-line)
    - [Filtering lines with patterns](#filtering-lines-with-patterns)
    - [Regular expressions](#regular-expressions)
  - [Task-based tools](#task-based-tools)
    - [Execute a command on a remote host](#execute-a-command-on-a-remote-host)
    - [Retrieve contents from URLs](#retrieve-contents-from-urls)
    - [Querying JSON data](#querying-json-data)
    - [Syncronizing files across hosts](#syncronizing-files-across-hosts)
    - [Run a command when a directory changes](#run-a-command-when-a-directory-changes)
  - [Writing programs](#writing-programs)
    - [Variables](#variables)
    - [Conditionals](#conditionals)
    - [Loops](#loops)
    - [Command line input](#command-line-input)
  - [Cheatsheet (commands in no particular order)](#cheatsheet-commands-in-no-particular-order)
  - [Pipe vs <](#pipe-vs-)
  - [Scripts](#scripts)
    - [Download all youtube links from a list to mp3](#download-all-youtube-links-from-a-list-to-mp3)

## Background
* Unix is a multiuser operating system where each user has its own private space on the machine’s harddisk and are identified by an id number.
* All users have a user name and a password and are required to enter them correctly before using the computer.
The user with id 0 is the system’s superuser or administrator and its traditional name is root.
* UNIX processes act on behalf of the initiating user.
* Multiple users can be running multiple processes at the same time.
* Users are organised in groups. A group is a set of users that share the same class of permissions.
* Everything in UNIX is represented by files. Everything can be:
  * Configuration files. Both user profile and system/server configuration files are plain text files. This allows to easily backup/restore/compare configuration files and remote administration using low-bandwidth text consoles.
  * Devices. Access is done using regular file I/O operations on filesystem objects (under /dev) that represent the real devices. For example cat file.wav >/dev/sndcard plays an audio file directly to the soundcard or cat file.txt\|lpr prints it to a printer.

## Filesystem
* The UNIX file system is the same in all Unix versions:

```
|--bin          Binaries required before mounting /usr
|--etc          System wide configuration files
|--home         Users’ home directories
|--lib          Libraries required by the system
|--tmp          Temporary files. Everyone has RW access.
|--usr          Programs
| |--bin        Programs’ executables
| |--lib        Programs’ libraries
| |--local      Programs that are install locally
| |   |-bin,lib,share
| |--share Programs’ required files (e.g. docs, icons)
| |--sbin  System administration programs
| |--src   Source files for the kernel and programs
|--var          Temporary space for running programs
```
 
### Permissions
```
$ ls -la /bin/ls
-rwxr-xr-x 1 root wheel   38624   Jul 15 06:29     /bin/ls
\        /     |      |       |   \          /     |
Permissions  Owner  Owning  Size     Date of     Filename
                    group          last modification
```

| Three permission triads | (with r: readable, w: writable, x: executable)
| ----------------------- |
| first triad             | what the owner can do |
| second triad            | what the group members can do |
| third triad             | what other users can do |

* The command `stat -c %a ./` will show the [numeric octal representation](https://en.wikipedia.org/wiki/File-system_permissions#Numeric_notation) of the premissions of `./` directory.
  * `ls -l ../` will show the permissions in the alphanumeric notation as in the first example together with all the other directories and files in the same level of `./`

* paths can be:
  * Absolute: starting from the root directory `/` e.g. `/var/log/messages` - The system log file
  * Relative to the current directory `.` e.g. if the current directory is `/var`, the relative path to the system log is `./log/messages` or `log/messages`

### File commands
* `ls`: list files in a directory
  * `ls -l`: list details
  * `ls -a`: list hidden files (files that start with .)
* `find <dir>`: walk through a file hierarchy starting from `<dir>`
  * `find <dir> -type [dfl]`: Only display directories, files or links
  * `find <dir> -name str`: Only display entries that start with str
  * `find <dir> -{max|min}depth d`
* `touch <file>`: Create and empty file named `<file>` or update the modification time for the existing file `<file>`
* `cp <from> <to>`: copy file or directory `<from>` to the location specified by `<to>`
  * -R: copy directories recursively
  * -p: preserve filesystem permissions and attributes
* `mv <from_1> · · · <from_n> <to>`: move files or directories `<from*>` to directory `<to>`
  * -n: do not overwrite existing files
* `mkdir`: create an empty directory in the path specified by the `.` The path to the directory must be pre-existing.
  * -p: also create intermediate directories as required

## Pipes \|
* Forward a program’s STDOUT line-by-line to the input of another program. This allows us to combine commands in surprising ways, using the pipe (\|) operator.
  * `cat file |wc -l`: Count lines of file
  * `find / -type d | sort`: View all directories sorted
  * `cat /var/log/access_log |grep foo|tail -n 10`: See the last 10 accesses from the host “foo” to our system’s web server
  * `cat /etc/passwd |cut -f1 -d':'|sort`: Get a sorted list of the system’s users.


## Text file processing commands
* `cat file_1 · · · file_n`: concatenate and print files to standard output (i.e. the terminal unless <kbd>&lt;</kbd> and <kbd>&gt;</kbd> are used determining other output)
  * send a program’s output to a file (>) or make a program read from a file (<)
  * (>) Overwrites an existing file; to append, we use (>>)
* `less file`: displays a file on the screen allowing to browse it on both directions.
  * q exit
  * /pattern search for pattern in text, pressing / repeatedly moves through all occurrences.
* `echo <string>` write arguments to standard output
* `head, tail <file>`: display first/last lines of `<file>`
  * -n Number of lines to display
  * -f Display newly appended lines
* `tr` character translator; can convert or delete specific characters
  * -s: replace repeating characters into
  * -d: delete a character

```bash
$ echo "foo bar" | tr 'o' 'a'
faa bar
# Replace tabs with spaces
$ tr '\t' ' ' < file.txt
# Remove all instances of #
$ tr -d '#' < file.txt
```

* `cut` allows us to split a line into columns, given a character, and extract specific fields.

```bash
# Get a list of users and home directories
$ cut -f1,6 -d: /etc/passwd
# Get details for all users that are running java
$ ps -ef|tr -s ' '|grep "java"|cut -f3,11  -d' '
```

* `sed` Modifies a string at its input in various ways using pattern matching

```bash
# Replace foo with bar in the input file
$ sed -e 's/foo/bar/' < file.txt

# Change the order of columns in a 2 column file
$ sed -e 's/^\(.*\) \(.*\)$/\2 \1/' < file.txt

# Remove lines 3 and 5 from the input
$ sed -e '3d' -e '5d' < book.txt
```

* `sed` is a domain specific language of its own. You can find a thorough manual [here](https://www.computerhope.com/unix/used.htm).

### Sorting data
* `sort` writes a (lexicographical) sorted concatenation of all input files to standard output, using Mergesort
  * `-r`: reverse the sort
  * `-n`: do a numeric sort
  * `-k` and `-t`: merge by the nth column (argument to `-k`). `-t` specifies what is the separator character

* `uniq` finds unique records in a sorted file

```bash
# Print the 10 most used lines in foo
$ cat foo| sort | uniq -c |sort -rn |head -n 10

# Sort csv file by the 6 field
sort -n -k 6 -t ','  datasets/file.csv
```

### Joining data
* `join` joins lines of two sorted files on a common field
  * -1, -2 specify fields in files 1 (first argument) and 2 (second argument) that represent keys

```bash
$ cat foodtypes.txt
3 Fat
1 Protein
2 Carbohydrate

$ cat foods.txt
Potato 2
Cheese 1
Butter 3

join -1 1  -2 2 <(sort foodtypes.txt) <(sort -k 2 foods.txt)
```

* Practically, join performs a join operation on KV pairs.

## Process management
* UNIX can do many jobs at once, dividing the processor’s time between the tasks so quickly that it looks as if everything is running at the same time. This is called multitasking.
* The UNIX shell has process management capabilities. When running a process, pressing Ctrl+Z will suspend it.
* A process can be killed with Ctrl+C
* A process can be started at the background by appending a & after the command. i.e. find / \|sort &
* `ps` See which processes are running

```
 UID   PID  PPID   C STIME   TTY           TIME CMD
    0     1     0   0 28Nov17 ??        21:20.80 /sbin/launchd
    0    51     1   0 28Nov17 ??         0:41.63 /usr/sbin/syslogd
    0    52     1   0 28Nov17 ??         2:19.32 /usr/libexec/UserEventAgent (System)
```

* `kill -<singalno> <pid>`: Send a signal to a process Important signals:
  * TERM: informs the process that it should terminate.
  * KILL: directly kill a process

## Documentation
* UNIX is a self-documenting system. All commands/tools have a manual page that describes their arguments, input and output formats and sometimes, even programming interfaces. `man <cmd>` invokes the manual for a command.

```
$ man ls
LS(1)                            User Commands                           LS(1)

NAME
       ls - list directory contents

SYNOPSIS
       ls [OPTION]... [FILE]...

DESCRIPTION
       List  information  about  the FILEs (the current directory by default).
       Sort entries alphabetically if none of -cftuvSUX nor --sort  is  speci-
       fied.
```

* Unix systems are also traditionally documented by providing full access to the source code that comprises them.

## Running a command per input line
* `xargs cmd` will run cmd on each line in STDIN

```bash
# Get file size statistics for the current directory
$ find . -type f -maxdepth 1 |xargs wc
```

* xargs by default appends each line at the end of cmd. Some times, it may be necessary to append it in the middle. We use the -I {} option and

```bash
$ find . -type f -maxdepth 1|xargs -I {} echo File {} is in `pwd`
File ./labcontents.doc is in /Users/gousiosg/Documents/course-material/isrm
File ./Makefile is in /Users/gousiosg/Documents/course-material/isrm
[...]
```

* xargs can process things in parallel with -P option.
* In terms of data processing, xargs is the equivalent of map

### Filtering lines with patterns
* grep prints lines matching a pattern
  * -v: invert search result (only print those that DO NOT match the pattern)
  * -i: make matching case insensitive
  * -n: print the line number of the match
  * -R: recurse a directory structure

```bash
# Find all processes run by user 501
$ ps -ef | egrep "^ +501"

# Find all files that extend class Foo
$ grep -Rn "(Foo)" * | grep *.py

# Same, more efficient
$ find . -type f -name '*.py' | xargs grep -n "(Foo)"

# Even more efficient
$ grep -Rn "(Foo)" *.py
```

### Regular expressions
* `.` Match any character once
* `*` Match the previous pattern 0 or more times
* `+` Match the previous pattern 1 or more times
* `[e-fF-M]` Match any character in the (ASCII) range F-M or e-f
* `[^e]` Match all characters except e
* `^ and $` Match the beginning or the end of the line, respectively
* `()` Group together items for future reference
* `|` Match either the left or the right group

## Task-based tools
### Execute a command on a remote host
* `ssh` provides a way to securely login to a remote server and get a prompt. In addition, it enables us to remotely execute a command and capture its output

```bash
# List of files on host dutihr
ssh dutihr ls
```

* Firewall piercing / tunneling: We can use ssh to access ports on machines where a firewall blocks them

```bash
# Connect to port
$ ssh -L 27017:mongoserver:27017 mongoserver

# On another terminal
$ mongo localhost:27017
```

### Retrieve contents from URLs
* `curl` queries a URL and prints the raw contents on the terminal
  * -H Set an HTTP header, e.g. “Authorization: token OAUTH-TOKEN”
  * -i Display all headers received
  * -s Don’t anything except from the response

```bash
curl -i "https://api.github.com/repos/vmg/redcarpet/issues"
```

* We can then process contents with a pipeline

```bash
# Get all magnet links from a page
curl -s https://thepiratebay.org/browse/101 | # Get contents
tidy 2>/dev/null |                            # Tidy up HTML
grep magnet\:\? |                             # Only get links
tr -d '"'                                     # Remove quotes
```

### Querying JSON data
* `json_pp` pretty-prints JSON files
* `jq` uses a Domain Specific Language (DSL) to query tree structures in JSON files.

```bash
# Extract information for a Cargo package descriptor
curl -s "https://crates.io/api/v1/crates/libc" |
jq -M '[.crate .id, .crate .repository, .crate .downloads|tostring]|join(", ")'


"libc, https://github.com/rust-lang/libc, 7267424"
```

### Syncronizing files across hosts
* `rsync` can be used to sync files between directories
  * -a archive mode, preserve permissions and access times
  * -v display files changed
  * --delete


### Run a command when a directory changes
* `inotifywait` watches a directory for changes and prints a log of the changes
  * -m enables monitor mode (run forever)
  * -r watch directories recursively

```bash
## See changes to your database files
inotifywait -mr --timefmt '%d/%m/%y %H:%M' --format '%T %w %f' /mongo

## Copy all new files in the current directory to another location
inotifywait -mr . |
grep CLOSE_WRITE |
cut -f1 -d' ' |
xargs -I {} cp {} /tmp
```

## Writing programs
* Bash is the name of the default command interpreter on most Unix environments. Bash is an almost complete programming language, with an interesting caveat: Many of its operators are programs that can be run individually

### Variables
* Variables in bash are strings followed by `=`, e.g. `cwd="foo"` and are dereferenced with `$`, e.g. echo `$cwd`.

```bash
# Store the results of running ls in a variable
listing=`ls -la`
echo $listing
```
 * An interesting set of variables are called environment variables. Those are declared by the operating system and can be read by all programs. The user can modify them with the export program.

```bash
$ export |grep PATH
$ export PATH=$PATH:/home/gousiosg/bin
$ export |grep PATH
```

### Conditionals
* Bash supports if / else blocks

```bash
if [ -e 'test' ]; then
  echo "File exists"
else
  echo "File does not exist"
fi
```

* `[` is an alias to the program test.
  * `[ $foo = 'test' ]`: Tests string equality
  * `[ $num -eq 3 ]`: Tests number equality
  * `[ ! expression ]`: Negates the expression

### Loops

* The `for` loop iterates over all items in the list provided as argument:

```bash
# Print 1 2 3 4...
for i in `seq 1 10`; do
  echo $i
done

# Iterate over all files in a directory
for i in $(ls); do
  echo `file --mime $i`
done
```

* `while` executes a piece of code if the control expression is true

```bash
ls -fa |tr -s ' '|cut -f9 -d' '|
while read file; do
  echo `file --mime $file`
done
```

### Command line input

* bash maps special variables on command line inputs: `$0` is the program name, `$1` is the first argument, `$2` the second etc. More complex command lines (e.g. with switches) can be done with getopt.

```bash
#!/usr/bin/env bash

argA="defaultvalue"
while getopts ":a" opt; do
  case $opt in
    a)
      echo "-a was triggered!" >&2
      argA=$OPTARG
      ;;
    \?)
      echo "Invalid option: -$OPTARG" >&2
      ;;
  esac
done
```

* The program above also illustrates the use of `case`


## Cheatsheet (commands in no particular order)
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
Legend for some commands (do not include the brackets) : [optional field], &lt;mandatory field&gt; #comments.
The dollar sign indicates that you are not running the command as 'root' user (user that can change "kernel" components i.e. brightness, firewall, hardware stuff...).

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

Sources: [Gousios' Unix Summary](https://gousios.org/courses/bigdata/ds-cmd-line.html) and [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/2020/course-shell/)

## Pipe vs <
```bash
sergio@hp:~/cg$ cat fib.c
#include <stdio.h>

int main(void) {
  int x, y, z;

  
    x = 0;
    y = 1;
    do {
      printf("%d\n",x);

      z = x + y;
      x = y;
      y = z;
    } while (x < 255);
  
}
sergio@hp:~/cg$ ./fib | head -n5 > head.txt # Taks output of ./fib as input for program head, then program output is sent to a file (overwrites it)
sergio@hp:~/cg$ cat head.txt
0
1
1
2
3
sergio@hp:~/cg$ ./fib <  head -n5 > head.txt # Takes file "head" as input for program ./fib (program fib ignores any input anyway. Note that we no longer run head program). Then output ./fib to head.txt
sergio@hp:~/cg$ cat head.txt
0
1
1
2
3
5
8
13
21
34
55
89
144
233
sergio@hp:~/cg$ ./fib | head -n6 | tee  head.txt | wc -l # Tee stores input to a file but also keeps it for other uses (it can be passed with pipes to another program)
6
sergio@hp:~/cg$ cat head.txt
0
1
1
2
3
5
sergio@hp:~/cg$ 


```

* *I have an empty file named "head" to avoid error message in the last example of wrong command

## Scripts

### Download all youtube links from a list to mp3

```bash
#!/usr/bin/env bash
# requires to have installed youtube-dl and ffmpeg

videos=$(cat list.txt)

for i in $videos; do
    snap run youtube-dl --extract-audio --audio-format mp3 $i || echo $i >> errors.txt
done
```