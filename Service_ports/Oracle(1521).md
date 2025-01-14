## Quick Intro

Oracle database (Oracle DB) is a relational database management system (RDBMS) from the Oracle Corporation (from [here](https://www.techopedia.com/definition/8711/oracle-database)).

When enumerating Oracle the first step is to talk to the TNS-Listener that usually resides on the default port (1521/TCP, -you may also get secondary listeners on 1522–1529-).

## Nmap Script

```
nmap -p 1521 -A $ip

nmap -n -v -sV -Pn -p 1521 –script=oracle-enum-users –script-args sid=ORCL,userdb=users.txt $ip

nmap --script "oracle-tns-version" -p 1521 -T4 -sV <IP>
# TNS listener version

nmap --script=oracle-sid-brute $ip
nmap  -n -v -sV -Pn -p 1521 --script=oracle-brute $ip
# Brute-Force Account
```

## oscanner

```
oscanner -s $ip -P 1521
```

## Fingerprint oracle tns

tnscmd10g = A tool to prod the oracle tnslsnr process

```
tnscmd10g version -h 192.168.1.101
tnscmd10g status -h 192.168.1.101
```

Other useful TNS listener commands:


| **Command** | **Purpose**                                                     |
| ----------- | --------------------------------------------------------------- |
| ping        | Ping the listener                                               |
| version     | Provide output of the listener version and platform information |
| status      | Return the current status and variables used by the listener    |
| services    | Dump service data                                               |
| debug       | Dump debugging information to the listener log                  |
| reload      | Reload the listener configuration file                          |
| save_config | Write the listener configuration file to a backup location      |
| stop        | Invoke listener shutdown                                        |

If you **receive an error**, could be because **TNS versions are incompatible** (Use the `--10G` parameter with `tnscmd10`) and if the **error persist,** the listener may be **password protected** (you can see a list were all the [**errors are detailed here**](https://docs.oracle.com/database/121/ERRMG/TNS-00000.htm#ERRMG-GUID-D723D931-ECBA-4FA4-BF1B-1F4FE2EEBAD7)) — don't worry… hydra to the rescue**:**

```
hydra -P rockyou.txt -t 32 -s 1521 host.victim oracle-listener
```