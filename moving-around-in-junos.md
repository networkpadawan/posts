Title: Moving Around in JunOS
Author: Tiago Sousa
Date: 2011-06-13 17:29:10
Tags: Certification, JNCIA, Juniper


I been studying for the JNCIA-JUNOS exam, and decided to post a little 101 of this OS, because if in some cases it is very similar to IOS, in others it´s a different kind of soup.

Should be a foot note but I'll say it up front, if you're reading and have any corrections to ask/add please feel free to do so in the comments area!



**JunOS Architecture**

JunOS provides a modular architecture, based on FreeBSD, with separated control and forwarding planes.

Control Plane - Routing Engine

Forward Plane - Packet Forwarding Engine

All routing protocols run on the control plane, and the control plane maintains routing tables used to build

forwarding tables and these are received by the forwarding plane to forward packets. Got it?



**The JunOS CLI**

The CLI has 2 modes

Operational mode - monitor and troubleshoot ** **

Configuration mode - configure everything in JunOS ** **

If you´re logged as root from the shell type **_cli _**to go to **operational-mode ( >) **and after that type **_configure _**to enter** configuration mode ( [edit] # )**


## Commands:










**Command**


**Result**






?


display option of current command






help


context help






edit


function like cd command in linux






spacebar


complete a command






tab


complete a command /variable






up


move up 1 level in hierarchy






up _N_


move up _N _levels in hierarchy






top


move to the top of the hierarchy






exit


move t previous higher level hierarchy/exits configuration mode






delete


functions like IOS no command






wildcard delete


delete several similar configurations (like addresses in interfaces)






|


functions exactly like pipe in linux






active/deactive


ignore part of the config like # in bash






JunOS  also provides a gui called J-Web, for basic configurations and troubleshooting...



**Initial Configuration Checklist**

When you power up a JunOS device make sure you configure the following:**
**



	
  * Hostname  - s_et hostname R1_



	
  * System Time -_ set time-zone Europe -/-_



	
  * Remote Access (Telnet/SSH) - _set services telnet / ssh_



	
  * Management interface and default route for management traffic


Extra configurations you can add:

Timeout -_ set cli idle-timeout 0/60_
MOTD - _set login message “ welcome”_
NTP- _set date ntp _“ntp address”

Remember: A root passsword is ALWAYS required before activating any changes to the configuration file

to set it up > _set system root-authentication plain-text password_

and if you forget that password, the way to recover is:

boot the system with "boot -s" flag
type_ recovery_
_set root pwd_
_type commit _and reboot



**JunOS Configuration Structure**

Active configuration - Currently operational configuration

Candidate Configuration - Editable configuration that might become active, but  until committed, as no impact on the device. (eq. Cisco running-config)

Configure Exclusive -Locks the candidate configuration to the user who issues the command

Configure Private - Give a copy of the candidate configuration

Rescue Configuration - This allows you to define a known working configuration

To save the configuration

_request system configuration rescue save_

to use the configuration at any time:

_rollback rescue_



**How to submit configuration to active status?**

_Commit at time_ - schedule commit configuration

_Commit and-quit_ - active candidate configuration and exit configuration mode

_Commit comment “string”_ - adds log entry to commit

_Commit confirmed X_ (default 10 minutes) -  commits the config, but if after 10 minutes, another commit is not issued, it rollbacks to previous active configuration. 

_Commit check_ - checks configurations without commiting 

_Rollback _- rollback to one of 50 candidate configuration (after issue commit command)

_Show | compare _- Displays  differences between active and candidate connfigurations 

_Show | compare rollback X_ - uses unix diff style comparison between candidate and rollback X

Batch Configuration Changes - In JunOS it´s possible to group several configurations and apply them at the same time, to do so issue:

_load patch terminal|filename path
_



_ _**Auto Backup of Configuration**

At the [edit system archival] add the option:

_transfer-interval interval _- Transfers the Active configuration at periodical intervals


_transfer-on-commit _- Transfers the current active configuration when a configuration is committed




**Junos Naming Format**


jinstall-10.1R1.8-domestic-signed.tgz




|              |                  |




package-release-edition


Package - “for which plataform the OS is”
Release - the release number of major and minor releases (a corrigir)
Edition - domestic(strong encryption or export



**Upgrading JunOS**

To upgrade a JunOS device to a new version dowload the .tgz and issue the command

_request system software add path_ image name



**Unified ISSU**

In JunOS, you're  able to upgrade between two JunOS releases without disruption.

It´s only availabe on dual routing engine plataforms and (GRES) Graceful Routing Engine Switchover and (NSR)Nonstop Active Routing must be enabled

type_ request software in-service-upgrade_ on master RE

__

Next I´ll talk about the [JunOS Interfaces](http://www.study.networkpadawan.com/2011/06/13/junos-interfaces).











## Junos OS architecture




- modular, based on freebsd,  separated control and forwarding planes

all routing protocols run on the control plane,
the control plane mantains routing tables used to build forwarding tables
forwarding tables are received by the forwarding lpane

Configuration



Active configuration - currently operational config.

Candidate Configuration - editable configuration that might become active.

## config exclusive and private - to be defined

how to submit configuration ?

commit at time - schedule commit configuration
commit and-quit - activate candidate configuration and exit conf mode
rollback - rollback to one of 50 candidate configuration (after issue commit command)
use show | compare to display differences between current active and candidate configs
commit comment “string” . add log entry to commit



Junos Naming Format


jinstall-10.1R1.8-domestic-signed.tgz




|              |                  |


package-release-edition

package - “for which plataform the OS is”
release - the release number of major and minor releases (a corrigir)
edition - domestic(strong encryption or export

**Upgrading JunOS**

> request system software add path/image name

> unified issu - only availabe on dual routing engine plataforms

Auto update feature

on [edit system archival]

options - transfer-interval - transfer-on-commit

(GRES) Graceful Routing Engine Switchover and (NSR)Nonstop Active Routing must be enabled

