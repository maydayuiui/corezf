+++
thumbnail = ""
tags = ["unix"]
categories = ["DevOps"]
date = "2017-02-08T14:53:03+08:00"
description = "在 Unix 中，如何查看一个文件在哪包中以及一个包中包含哪些文件"
title = "Unix 中，查找文件所属的包以及包包含的文件"
+++

在使用 Unix 系统的过程，经常会遇到查询一个文件是通过哪个包安装进来的以及一个包中包含了哪些文件的情况。下面列举一些常用命令来解决这个问题。

> 这里只讨论已经安装的包。

#### Solaris 11

```
// Find gzip in which package
root@solaris11:~# pkg search -l /usr/bin/gzip
INDEX      ACTION VALUE        PACKAGE
path       file   usr/bin/gzip pkg:/compress/gzip@1.5-0.175.3.0.0.30.0

// Find package pkg:/compress/gzip contain which files.
root@solaris:~# pkg contents pkg:/compress/gzip
PATH
usr/bin/gunzip
usr/bin/gzcat
usr/bin/gzcmp
usr/bin/gzdiff
usr/bin/gzegrep
usr/bin/gzexe
usr/bin/gzfgrep
usr/bin/gzforce
usr/bin/gzgrep
usr/bin/gzip
usr/bin/gzless
usr/bin/gzmore
usr/bin/gznew
usr/share/info/gzip.info
usr/share/man/man1/gunzip.1
usr/share/man/man1/gzcat.1
usr/share/man/man1/gzcmp.1
usr/share/man/man1/gzdiff.1
usr/share/man/man1/gzegrep.1
usr/share/man/man1/gzexe.1
usr/share/man/man1/gzfgrep.1
usr/share/man/man1/gzforce.1
usr/share/man/man1/gzgrep.1
usr/share/man/man1/gzip.1
usr/share/man/man1/gzless.1
usr/share/man/man1/gzmore.1
usr/share/man/man1/gznew.1
```

#### Solaris 10 and old version Solaris

```
// Find gzip in which package
# pkgchk -l -p  /usr/bin/gzip
Pathname: /usr/bin/gzip
Type: regular file
Expected mode: 0555
Expected owner: root
Expected group: bin
Expected file size (bytes): 141836
Expected sum(1) of contents: 7568
Expected last modification: Aug 25 09:29:45 2015
Referenced by the following packages:
        SUNWgzip
Current status: installed

// Find package SUNWgzip contain which files.
# pkgchk -l SUNWgzip

```

#### Redhat, CentOS, Fedora

```
// Find gzip in which package
[root@centos ~]$ rpm -qf /bin/gzip
gzip-1.3.12-18.el6.x86_64

// Find package gzip contain which files.
[root@centos ~]$ rpm -ql gzip-1.3.12-18.el6.x86_64
/bin/gunzip
/bin/gzip
/bin/zcat
/usr/bin/gunzip
/usr/bin/gzexe
/usr/bin/gzip
/usr/bin/zcmp
/usr/bin/zdiff
/usr/bin/zegrep
/usr/bin/zfgrep
/usr/bin/zforce
/usr/bin/zgrep
/usr/bin/zless
/usr/bin/zmore
/usr/bin/znew
/usr/share/doc/gzip-1.3.12
/usr/share/doc/gzip-1.3.12/AUTHORS
/usr/share/doc/gzip-1.3.12/ChangeLog
/usr/share/doc/gzip-1.3.12/NEWS
/usr/share/doc/gzip-1.3.12/README
/usr/share/doc/gzip-1.3.12/THANKS
/usr/share/doc/gzip-1.3.12/TODO
/usr/share/info/gzip.info.gz
/usr/share/man/man1/gunzip.1.gz
/usr/share/man/man1/gzexe.1.gz
/usr/share/man/man1/gzip.1.gz
/usr/share/man/man1/zcat.1.gz
/usr/share/man/man1/zcmp.1.gz
/usr/share/man/man1/zdiff.1.gz
/usr/share/man/man1/zforce.1.gz
/usr/share/man/man1/zgrep.1.gz
/usr/share/man/man1/zless.1.gz
/usr/share/man/man1/zmore.1.gz
/usr/share/man/man1/znew.1.gz
```
