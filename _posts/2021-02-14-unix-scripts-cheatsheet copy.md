---
layout: post
title:  "Unix scripts cheat sheet"
date:   2021-01-31 12:00:00 +0100
categories: linux
tags: terminal
---
<!--more-->

Source: [The Missing Semester of Your CS Education](https://missing.csail.mit.edu/2020/course-shell/)

You can assign variable values with <kbd>=</kbd> but there must be no spaces surrounding the operator. Otherwise you'd be running a program with `=` as second argument:
```console
$ foo = bar
Command 'foo' not found
$ foo=bar
$ echo $foo
bar
$ echo "Hello $foo"
Hello bar
$ echo 'Hello $foo'
Hello $foo
```

To be continued...