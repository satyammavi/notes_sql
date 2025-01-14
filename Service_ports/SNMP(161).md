## Quick Intro

- The Simple Network Management Protocol (SNMP) is a protocol **used in TCP/IP networks to collect and manage information about networked devices.** SNMP operates in the application layer (layer 7 of the OSI model) and uses **UDP port 161** to listen for requests. The SNMP protocol is supported by many types of devices including routers, switches, servers, printers, Network Attached Storage (NAS), firewalls, WLAN controllers and more. In the following sections we will be looking at the main components of SNMP managed networks, how they communicate with each other and something called the Management Information Base (MIB). We will also look at how and why SNMP can cause security issues and, of course, how to enumerate the SNMP protocol.
    

***SNMP components -***

SNMP managed networks have 3 components:

1. Managed Device A managed device (also referred to as a ‘node’) is a network device with the SNMP service enabled allowing unidirectional (read) or bidirectional (read/write) communication. Managed devices can be any networked device including servers, firewalls and routers.
    
2. Agent The agent is the software running on the managed device which is responsible for handling the communication. The agent translates device-specific configuration parameters into an SNMP format for the Network Management System.
    
3. Network Management System (NMS) The Network Management System is the software that is actually managing and monitoring networked devices. An SNMP managed network will always contain at least one NMS.
    

***SNMP commands -***

The SNMP protocol uses several commands which are sent from the NMS to the managed device’s agent and back. These commands can be categorized as read, write, trap and traversal commands.

- Read commands are sent by the NMS to nodes for monitoring purposes.
    
- Write commands are used to control the nodes in the network.
    
- The trap commands are used for unsolicited SNMP messages from a device’s agent to the NMS to inform the NMS about certain events such as errors.
    
- Traversal commands are used to check what information is retained on a managed device and to retrieve it.
    

***SNMP Management Information Base (MIB) -***

- The SNMP Management Information Base (MIB) is a database that contains information about the network device. When the Network Management System (NMS) sends a ‘get’ request for information about a managed device on the network, the agent service returns a structured table with data. This table is what is called the Management Information Base (MIB). MIB values are indexed using a series of numbers with dots. For example, MIB value 1.3.6.1.2.1.1.1 refers to the system description (sysDescr) and value 1.3.6.1.2.1.1.6 refers to the system location (sysLocation).
    

***SNMP Community strings -***

- The SNMP community string is like a username or password that allows access to the managed device. There are three different community strings that allow a user to set (1) read-only commands, (2) read and write commands and (3) traps. Most SNMPv1 and SNMPv2 devices ship from the factory with a default read-only community string set to **‘public’** and the read-write string set to ‘private’. As these default values are well-known and easy to guess, it is good security practice to replace all community strings with a value that is hard to guess. It is good practice to threat community strings as passwords. In SNMPv3, the community string was replaced by username and password authentication.
    

## SNMP MIB Trees

- 1.3.6.1.2.1.25.1.6.0 - System Processes
    
- 1.3.6.1.2.1.25.4.2.1.2 - Running Programs
    
- 1.3.6.1.2.1.25.4.2.1.4 - Processes Path
    
- 1.3.6.1.2.1.25.2.3.1.4 - Storage Units
    
- 1.3.6.1.2.1.25.6.3.1.2 - Software Name
    
- 1.3.6.1.4.1.77.1.2.25 - User Accounts
    
- 1.3.6.1.2.1.6.13.1.3 - TCP Local Ports


## SNMPwalk

SNMPwalk is a great tool to query MIB values to retrieve information about managed devices, but, as a minimum, **it requires a valid SNMP read-only community string.**


```
for community in public private manager; do snmpwalk -c $community -v1 $ip; done
# here it will take three comunity strings and check one by one

snmpwalk -c public -v1 $ip

snmpwalk -c public -v2c <target-ip>
# here -c stands for community string and 2c is most common version found on today's snmp devices
```

## SNMPcheck

Same as `snmpwalk` but give nice output

```
snmpcheck -t 192.168.1.X -c public
```

## Brute forcing community string

### OneSixtyOne

- Onesixtyone is a very fast tool to brute force SNMP community strings and take advantage of the connectionless protocol. Onesixtyone sends an SNMP request and (by default) waits 10 milliseconds for a response. If the community string sent by onesixtyone to the SNMP enabled device is invalid, then the request is dropped. However, if a valid community string is passed to an SNMP enabled device, the device responds with the information requested (the ‘system.sysDescr.0’ value).

```
onesixtyone -c dict.txt <ip>
```



## Wordlists

```
/usr/share/seclists/Discovery/SNMP/common-snmp-community-strings-onesixtyone.txt

/usr/share/metasploit-framework/data/wordlists/snmp_default_pass.txt
```

## NSE Script

```
ls -l /usr/share/nmap/scripts/snmp*
```

## NMPv3 Enumeration

```
wget https://raw.githubusercontent.com/raesene/TestingScripts/master/snmpv3enum.rb; ./snmp
```