Title: JunOS - Monitoring and User Authentication
Author: Tiago Sousa
Date: 2011-06-13 17:43:34
Tags: Certification, JNCIA, Juniper


## User Authentication


In JunOS each command or config is subject to authorization.

Root user or high level users can define authorization parameters.

**User Class**

A login class is a named container that contains a set of one or more permission flags.

Predifined classes:



	
  * super-user : all permissions



	
  * operator : clear ; network; reset; trace and view permissions



	
  * read-only : view permissions



	
  * unauthorized : no permissions


Classes can also define commands that override the permissions flags given, if they match exactly the override given.

_deny-command | configuration _- bypasses permissions and denys command|configuration
_allow-command | configuration_ - bypasses permissions and allows command|configuration

In JunOS you can use 3 types of authentication

**Local User Authentication**

Passwords must have 6 characters and must contain at least one change of case or character class

**RADIUS and TACACS+**

JunOS have both RADIUS and TACACS+ clients, and a locally defined user account determines authorization.

JunOS can use local and server auth at the same time(one at each time) to check the order of auth that is being used issue _show system authentication-order_


## Monitoring  and Troubleshooting  in JunOS


To obtain and monitor system and chassis

_show system alarms | boot-messages | connections | statitistcs | storage_

_show chassis alarms | enviroment | hardware | routing-engine_

the monitor traffic command enables the tcpdump utility. the tool monitors the traffic that originates or terminates on the RE. by default it monitors the management interface. by adding the layer2-headers, this tool also monitors l2 probles.

JunOS uses a unix syslog-style for system wide high level operations. it supports a number of facilities and severity levels. The facility is listed first and defines the class of the log, the severity is second and defines the level of detail.
Regular Log messages consist of:



	
  * Timestamp - date of log



	
  * Name - system name



	
  * Process name | PID - name or ID of process



	
  * Message-code - provides a code to help identify the issue



	
  * Message-text - gives additional info


remember : In JunOS debugging is called tracing

to view log files

show log file-name

to view logs in real time like tail -f

monitor start filename

you can add the | match statement to sort the displayed info

monitor stop stops monitoring

monitor interfacs

show interfaces descriptions show interfes with descriptions

show interfaces terse like "brief"

show interface fe-0/0/1 |brief|detail| extensive (from less to more ?!?

On serial links, to get line information add _detail _option and to get layer 2 errors add _extensive _option




## SNMP Support


JunOS has a SNMP Agent, this agent exchanges network management information with a SNMP Server/Manager.
SNMP Message types:



	
  * Get, Getbulk, Getnext request - SNMP server request information from  the SNMP agent, and the agent responds with a get response message



	
  * Set requests - SNMP Server changes the values of a MIB(Management  Information Base) object controlled by the agent. The agent acknoledges  with a set response message



	
  * Notifications - The SNMP agent sends traps to notify the server of  significant events regarding the network device. SNMPv3 uses informs" to  notify the manager of significant even


