--- 
name: ignoring-sudirectories-when-zipping-a-directory
layout: post
title: Ignoring subdirectories when using zip command (Linux)
time: 2020-11-05 10:41:00 +00:00
---

## The problem

Doing some build migration from Travis-CI to Github actions forced me to rewrite some packaging code and I stumbled on an issue with zipping up a directory and ignoring particular sub directories. i.e. I wanted to:

> Zip up package and ignore `node_modules`, etc.

Searching the interwebs suggested:

```bash
# this plain just didn't work
zip zippedfile.zip -r --exclude=path/to/subdir *
```

And:

```bash
# this worked, but was left with empty path/to/subdir directory
# directory in the zip
zip zippedfile.zip -r --exclude=path/to/subdir/**\* *
```

## The solution that worked for me

I could not find this solution in the man pages for zip. But instead, found it in a _comment_ in an [AskUbuntu post](https://askubuntu.com/questions/371579/how-to-exclude-directories-and-file-zipping-a-directory):

```bash
zip zippedfile.zip -r --exclude=\path/to/subdir/* *
```

To summarize, if you want to exclude a subdirectory, `${subdirectory}`, the syntax is either `-x\${subdirectory}/*` or `--exclude=\${subdirectory}/*`.