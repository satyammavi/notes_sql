## Quick Intro

- Used to send, receive, and relay outgoing emails
    
- Used port 25
    
- Main attacks are user enumeration and using an open relay to send spam
    
## NSE

```
nmap 192.168.1.101 --script=smtp* -p 25

nmap --script=smtp-commands,smtp-enum-users,smtp-vuln-cve2010-4344,smtp-vuln-cve2011-1720,smtp-vuln-cve2011-1764 -p 25 $ip
```

## User Enumeration

```
smtp-user-enum -M VRFY -U /usr/share/wordlists/metasploit/unix_users.txt -t $ip

for server in $(cat smtpmachines); do echo "******************" $server "*****************"; smtp-user-enum -M VRFY -U userlist.txt -t $server;done #for multiple servers
# For multiple servers
```

## Connection

```
telnet $ip 25
```

### 

[](https://gabb4r.gitbook.io/oscp-notes/service-enumeration/smtp-enumeration-port-25#command-to-check-if-a-user-exists)

Command to check if a user exists

Copy

```
VRFY root
```

### 

[](https://gabb4r.gitbook.io/oscp-notes/service-enumeration/smtp-enumeration-port-25#command-to-ask-the-server-if-a-user-belongs-to-a-mailing-list)

Command to ask the server if a user belongs to a mailing list

Copy

```
EXPN root
```

## Brute Force

```
hydra -P /usr/share/wordlistsnmap.lst $ip smtp -V
```



http://www.microhowto.info/howto/send_an_email_using_netcat.html