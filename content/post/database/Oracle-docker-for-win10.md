---
title: Use Oracle on Docker for Windows
date: 2020-10-20
categories: [Database]
tag: [Oracle, Docker]
---

| Environment | version |
| :---: | :---: |
| WSL | 2 |
| Docker Engine | v19.03.13 |
| Oracle Database | Enterprise 12.2.0.1 |

## Install WLS2

Check detailed and official document [here](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

### Problems I encountered

1\. Use any commands related to WSL2 will cause error： “The attempted operation is not supported for the type of object referenced.”

This is beacause the proxy software conflicts with the sock port of WSL2.

#### Short term solution

Run command line as admin:

``` powershell
netsh winsock reset
```

Then reboot computer.

#### Long term solution

Download NoLsp.exe [here](https://link.zhihu.com/?target=http%3A//www.proxifier.com/tmp/Test20200228/NoLsp.exe).

Run command line as admin:

``` powershell
<Location of NoLsp.exe> C:\windows\system32\wsl.exe
```

## Install Docker and Oracle

Download docker desktop and pull the image like in [this post](https://dbtut.com/index.php/2020/01/09/how-to-install-oracle-database-in-docker/).

## Use SQL Developer

Directly download and connection to the database.

### Problem I encountered

1\. Invalid username / password

I'm still a newbie to database. I still don't know why this happened. But the solution to this is to create a new user account in database system directly in docker. Then reboot and we can use the new accout to login.

Command to create a new user account:

``` SQL
SQL> alter session set "_ORACLE_SCRIPT"=true;
SQL> create user <username> identified by <password>;
SQL> GRANT CONNECT, RESOURCE, DBA TO <username>;
```
