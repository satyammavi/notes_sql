## Quick Intro

- DNS enumeration is the process of identifying the DNS servers and the corresponding DNS records. DNS stands for Domain Name System which is a database containing information about domain names and their corresponding IP addresses. The DNS system is responsible for translating human-readable hostnames into machine-readable IP addresses. The most important records to look for in DNS enumeration are the:
    
- A (address) records containing the IP address of the domain.
    
- MX records, which stands for Mail Exchange, contain the mail exchange servers.
    
- CNAME records used for aliasing domains. CNAME stands for Canonical Name and links any sub-domains with existing domain DNS records.
    
- NS records, which stands for Name Server, indicates the authoritative (or main) name server for the domain.
    
- SOA records, which stands for State of Authority, contain important information about the domain such as the primary name server, a timestamp showing when the domain was last updated and the party responsible for the domain.
    
- PTR or Pointer Records map an IPv4 address to the CNAME on the host. This record is also called a ‘reverse record’ because it connects a record with an IP address to a hostname instead of the other way around.
    
- TXT records contain text inserted by the administrator (such as notes about the way the network has been configured).
    
- The information retrieved during DNS enumeration will consist of details about names servers and IP addresses of potential targets (such as mail servers, sub-domains etc). Some tools used for DNS enumeration included with Kali Linux are: **whois, nslookup, dig, host and automated tools like Fierce, DNSenum and DNSrecon.** Let’s briefly review these tools and see how we can use them for DNS enumeration.
    

![](https://gabb4r.gitbook.io/~gitbook/image?url=https%3A%2F%2F3331885100-files.gitbook.io%2F%7E%2Ffiles%2Fv0%2Fb%2Fgitbook-legacy-files%2Fo%2Fassets%252F-MkfTlo0T97eXbWuX_cT%252F-MkgjSlLxvnvt7wiBGVG%252F-MkgsRBOFP_jtaMvh_sX%252Fimage.png%3Falt%3Dmedia%26token%3Dcddc213f-e643-4118-b522-af9d9d12f7ab&width=768&dpr=4&quality=100&sign=b829c465&sv=2)

general overview of different records

## Whois

```
whois <domain>
```

## Nmap

```
nmap -sC -sV -p53 $ip/24

nmap -p 80 --script dns-brute.nse domain.com
# Find DNS (A) records by trying a list of common sub-domains from a wordlist.

nmap $ip --script=dns-zone-transfer -p 53
# Zone transfer script
```

## Host

## Domain scan

```
$ host google.com

google.com has address 142.250.183.78
google.com has IPv6 address 2404:6800:4009:822::200e
google.com mail is handled by 20 alt1.aspmx.l.google.com.
google.com mail is handled by 10 aspmx.l.google.com.
google.com mail is handled by 50 alt4.aspmx.l.google.com.
google.com mail is handled by 40 alt3.aspmx.l.google.com.
google.com mail is handled by 30 alt2.aspmx.l.google.com.
```

## Find particular records

```
host -t mx google.com
# Return mail servers 

host -t ns google.com
# Return name servers 

host -t txt google.com
# Return txt records
```

## Reverse Domain Lookup

```
$ host gnu.org        

gnu.org has address 209.51.188.148
gnu.org has IPv6 address 2001:470:142:3::a
gnu.org mail is handled by 10 eggs.gnu.org.

$ host 209.51.188.148

148.188.51.209.in-addr.arpa is an alias for 148.0-24.188.51.209.in-addr.arpa.
148.0-24.188.51.209.in-addr.arpa domain name pointer wildebeest.gnu.org.
```

## DNS Zone Transfer

DNS zone transfer, also known as DNS query type AXFR, is a process by which a DNS server passes a copy of part of its database to another DNS server. The portion of the database that is replicated is known as a zone.

Copy

```
host -l <domain> <NameServer>
```

Copy

```
$ host -t ns zonetransfer.me               # first list out their name servers to check for zone transfer

zonetransfer.me name server nsztm2.digi.ninja.
zonetransfer.me name server nsztm1.digi.ninja.

$ host -l zonetransfer.me nsztm1.digi.ninja

Using domain server:
Name: nsztm1.digi.ninja
Address: 81.4.108.41#53
Aliases: 

zonetransfer.me has address 5.196.105.14
zonetransfer.me name server nsztm1.digi.ninja.
zonetransfer.me name server nsztm2.digi.ninja.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me domain name pointer www.zonetransfer.me.
asfdbbox.zonetransfer.me has address 127.0.0.1
canberra-office.zonetransfer.me has address 202.14.81.230
dc-office.zonetransfer.me has address 143.228.181.132
deadbeef.zonetransfer.me has IPv6 address dead:beaf::
email.zonetransfer.me has address 74.125.206.26
home.zonetransfer.me has address 127.0.0.1
internal.zonetransfer.me name server intns1.zonetransfer.me.
internal.zonetransfer.me name server intns2.zonetransfer.me.
intns1.zonetransfer.me has address 81.4.108.41
intns2.zonetransfer.me has address 167.88.42.94
office.zonetransfer.me has address 4.23.39.254
ipv6actnow.org.zonetransfer.me has IPv6 address 2001:67c:2e8:11::c100:1332
owa.zonetransfer.me has address 207.46.197.32
alltcpportsopen.firewall.test.zonetransfer.me has address 127.0.0.1
vpn.zonetransfer.me has address 174.36.59.154
www.zonetransfer.me has address 5.196.105.14
```

## Zone Transfer Script

```
#!/bin/bash
# Simple Zone Transfer Bash Script
# $1 is the first argument given after the bash script
# Check if argument was given, if not, print usage
if [ -z "$1" ]; then
echo "[*] Simple Zone transfer script"
echo "[*] Usage : $0 <domain name> "
exit 0
fi
# if argument was given, identify the DNS servers for the domain
for server in $(host -t ns $1 | cut -d " " -f4); do
# For each of these servers, attempt a zone transfer
host -l $1 $server |grep "has address"
done
```

## subdomain bruteforcing using common hostname

```
for ip in $(cat list.txt); do host $ip.website.com; done
```

## Reverse dns lookup bruteforcing

```
for ip in $(seq 155 190);do host 50.7.67.$ip;done |grep -v "not found"
```

The ip is based on subdomain bruteforcing result


## Nslookup

- nslookup is used to query Internet name servers interactively
  
```
$ nslookup hsploit.com

Server:     203.153.41.28
Address:    203.153.41.28#53

Non-authoritative answer:
Name:   hsploit.com
Address: 104.21.38.165
Name:   hsploit.com
Address: 172.67.136.119
Name:   hsploit.com
Address: 2606:4700:3033::6815:26a5
Name:   hsploit.com
Address: 2606:4700:3035::ac43:8877
```


## Running in intrective mode

```
$ nslookup

> set type=ns
> hsploit.com
Server:     203.153.41.28
Address:    203.153.41.28#53

Non-authoritative answer:
hsploit.com nameserver = dee.ns.cloudflare.com.
hsploit.com nameserver = jim.ns.cloudflare.com.

Authoritative answers can be found from:
> 
```



## Gathering information from specific DNS server

```
$ nslookup                                

> server 10.10.10.13
Default server: 10.10.10.13
Address: 10.10.10.13#53

> 10.10.10.13
13.10.10.10.in-addr.arpa    name = ns1.cronos.htb.
```

## Dig

## Domain scan

```
$ dig hsploit.com

; <<>> DiG 9.16.18 <<>> hsploit.com
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 13539
;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 0, ADDITIONAL: 1

;; OPT PSEUDOSECTION:
; EDNS: version: 0, flags:; udp: 1472
;; QUESTION SECTION:
;hsploit.com.           IN  A

;; ANSWER SECTION:
hsploit.com.        1200    IN  A   104.21.38.165
hsploit.com.        1200    IN  A   172.67.136.119

;; Query time: 139 msec
;; SERVER: 203.153.41.28#53(203.153.41.28)
;; WHEN: Thu Jul 22 17:04:58 IST 2021
;; MSG SIZE  rcvd: 72
```

## Asking for particular records

```
dig hsploit.com -t mx
```

## Sorting the output

```
$ dig hsploit.com -t ns +short 

dee.ns.cloudflare.com.
jim.ns.cloudflare.com.
```

Note - if particular type of information is not available , dig will **give no output** so don't think at that time that tool is not working xD

## Reverse Domain Lookup

```
$ dig -x 142.250.183.78 +short        

bom12s12-in-f14.1e100.net.
```

## Zone Transfer

```
$ dig zonetransfer.me ns +short 

nsztm2.digi.ninja.
nsztm1.digi.ninja.

$ dig axfr zonetransfer.me @nsztm1.digi.ninja

; <<>> DiG 9.16.18 <<>> axfr zonetransfer.me @nsztm1.digi.ninja
;; global options: +cmd
zonetransfer.me.    7200    IN  SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
zonetransfer.me.    300 IN  HINFO   "Casio fx-700G" "Windows XP"
zonetransfer.me.    301 IN  TXT "google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA"
zonetransfer.me.    7200    IN  MX  0 ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  10 ALT1.ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  10 ALT2.ASPMX.L.GOOGLE.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX2.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX3.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX4.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  MX  20 ASPMX5.GOOGLEMAIL.COM.
zonetransfer.me.    7200    IN  A   5.196.105.14
zonetransfer.me.    7200    IN  NS  nsztm1.digi.ninja.
zonetransfer.me.    7200    IN  NS  nsztm2.digi.ninja.
_acme-challenge.zonetransfer.me. 301 IN TXT "6Oa05hbUJ9xSsvYy7pApQvwCUSSGgxvrbdizjePEsZI"
_sip._tcp.zonetransfer.me. 14000 IN SRV 0 0 5060 www.zonetransfer.me.
14.105.196.5.IN-ADDR.ARPA.zonetransfer.me. 7200 IN PTR www.zonetransfer.me.
asfdbauthdns.zonetransfer.me. 7900 IN   AFSDB   1 asfdbbox.zonetransfer.me.
asfdbbox.zonetransfer.me. 7200  IN  A   127.0.0.1
asfdbvolume.zonetransfer.me. 7800 IN    AFSDB   1 asfdbbox.zonetransfer.me.
canberra-office.zonetransfer.me. 7200 IN A  202.14.81.230
cmdexec.zonetransfer.me. 300    IN  TXT "; ls"
contact.zonetransfer.me. 2592000 IN TXT "Remember to call or email Pippa on +44 123 4567890 or pippa@zonetransfer.me when making DNS changes"
dc-office.zonetransfer.me. 7200 IN  A   143.228.181.132
deadbeef.zonetransfer.me. 7201  IN  AAAA    dead:beaf::
dr.zonetransfer.me. 300 IN  LOC 53 20 56.558 N 1 38 33.526 W 0.00m 1m 10000m 10m
DZC.zonetransfer.me.    7200    IN  TXT "AbCdEfG"
email.zonetransfer.me.  2222    IN  NAPTR   1 1 "P" "E2U+email" "" email.zonetransfer.me.zonetransfer.me.
email.zonetransfer.me.  7200    IN  A   74.125.206.26
Hello.zonetransfer.me.  7200    IN  TXT "Hi to Josh and all his class"
home.zonetransfer.me.   7200    IN  A   127.0.0.1
Info.zonetransfer.me.   7200    IN  TXT "ZoneTransfer.me service provided by Robin Wood - robin@digi.ninja. See http://digi.ninja/projects/zonetransferme.php for more information."
internal.zonetransfer.me. 300   IN  NS  intns1.zonetransfer.me.
internal.zonetransfer.me. 300   IN  NS  intns2.zonetransfer.me.
intns1.zonetransfer.me. 300 IN  A   81.4.108.41
intns2.zonetransfer.me. 300 IN  A   167.88.42.94
office.zonetransfer.me. 7200    IN  A   4.23.39.254
ipv6actnow.org.zonetransfer.me. 7200 IN AAAA    2001:67c:2e8:11::c100:1332
owa.zonetransfer.me.    7200    IN  A   207.46.197.32
robinwood.zonetransfer.me. 302  IN  TXT "Robin Wood"
rp.zonetransfer.me. 321 IN  RP  robin.zonetransfer.me. robinwood.zonetransfer.me.
sip.zonetransfer.me.    3333    IN  NAPTR   2 3 "P" "E2U+sip" "!^.*$!sip:customer-service@zonetransfer.me!" .
sqli.zonetransfer.me.   300 IN  TXT "' or 1=1 --"
sshock.zonetransfer.me. 7200    IN  TXT "() { :]}; echo ShellShocked"
staging.zonetransfer.me. 7200   IN  CNAME   www.sydneyoperahouse.com.
alltcpportsopen.firewall.test.zonetransfer.me. 301 IN A 127.0.0.1
testing.zonetransfer.me. 301    IN  CNAME   www.zonetransfer.me.
vpn.zonetransfer.me.    4000    IN  A   174.36.59.154
www.zonetransfer.me.    7200    IN  A   5.196.105.14
xss.zonetransfer.me.    300 IN  TXT "'><script>alert('Boo')</script>"
zonetransfer.me.    7200    IN  SOA nsztm1.digi.ninja. robin.digi.ninja. 2019100801 172800 900 1209600 3600
;; Query time: 133 msec
;; SERVER: 81.4.108.41#53(81.4.108.41)
;; WHEN: Thu Jul 22 17:28:02 IST 2021
;; XFR size: 50 records (messages 1, bytes 1994)
```

## Automated Scanners

### Fierce
```
fierce --domain google.com
```

### Dnsenum

```
dnsenum $ip

dnsenum google.com -f /usr/share/dnsenum/dns.txt
# Brute forcing subdomains
```

### DnsRecon
```
dnsrecon -d $ip

dnsrecon -d $ip -t axfr
# Perform zone transfer

dnsrecon -d $ip -D /usr/share/dnsrecon/subdomains-top1mil-20000.txt -t brt
# Perform host and subdomain brute force
```

## sub-Domain Enumeration

### ffuf

```
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-110000.txt -H "Host: FUZZ.horizontall.htb" -u http://horizontall.htb
```

### Sublist3r
```
sublist3r -d <domain>
# To scan with public data

sublist3r -d <domain> -b -t 100
# To bruteforce the subdomains
# this will use following wordlist:
    /usr/share/sublist3r/subbrute/names.txt
```