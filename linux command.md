 

# command 
## use full command for me 
## for education 

1.hostname : proved a hostnme of the target machine.
2. uname -a : he is giving additial detail of target KERNEL
2.1. uname -r : to get which linux we use
3. ps : its use for see a running procces on a linux
3.1 ps -A : view all run proceses
3.2 ps axjf : view process tree
3.3 ps aux : user use processes
4. env : will show environment variable
5. netstat -a : show all listening ports and established connection
6. netstat -at : con also be used  to list tcp or udp protocal
7. netstat -l : list ports in listening mode
8. netstat -s : list network usage statistic by protocol
9. netstat -tp : list connection with the service name and pdi information
10. netstat -a : display all sockets
11. netstat -n : do not reslove name
12. netstat -o : display timer

13. find / -type f -name text.txt : he find this file in all directory 
14 find /home -type f -name text.txt : he find this file in only home  




 # File use
?
1. /proc/version: provide information about the target system processes and may be give KERNEl version information
2. /etc/issue : provide a operting system information and can we easily modife and change the information
3. /etc.passwd : this file store a all user and there password
3.1 cat /etc/passwd | grep home : to find real user 
3.2 cat /etc/shadow
