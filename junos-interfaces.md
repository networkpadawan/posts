Title: JunOS - Interfaces
Author: Tiago Sousa
Date: 2011-06-13 17:31:20
Tags: Certification, JNCIA, Juniper


How to _read _a interface:

**ge-0/2/3** = **port 3** of Gigabit Ethernet PIC in **slot 2** of **FPC 0**
*FPC - Flexible PIC Concentrator = Line Card

Types of Interfaces:

**Ethernet interfaces**

Fast and Giga Ethernet Interfaces auto negotiate the parameters as needed such as speed and duplex format

Ethernet Aggregated interfaces

As defined by 802.3ad, link bonding serves the purpose of adding multiple physical links to a virtual interface.

How to create agregated Ethernet interfaces:

_set __Ethernet device-count X_ -  created interface aeX

_set interfaces "interface" fastether-options 802.3ad "aeX"_ - add interfaces to agregated interface

_set interface aeX unit 0 family inet address 1.1.1.1 - _config the virtual interface



**Serial Interfaces**

Serial ports - (Serial, E1,E3,T1,T3,SONET/SDH)  have separate configuration values for each type of physical interface (ex.: t1-options ; e1-options)

Features:



	
  * Loopback - serial interfaces ignore (by default) loopback requests, but can be changed for t1 and t3 interfaces




	
  * Payload scrabling is also disabled by default on t3/e3 interfaces, but can be enabled. like wise, on sonet/sdh interfaces it is enabled by default but can be disabled




	
  * Checksum - 16 bit by default but it can be 32 bit by adding the _fcs 32_ option




	
  * L2 encapsulation - uses ppp by default but can also be Frame Relay or Cisco HDLC




**Special Interfaces**

lo0 - loopback interface
fxp0 Out-of-band FE interface for M/T/MX Series routers** **



**Interface Units**

Units in JunOS function just like Cisco IOS sub-interfaces  but in JunOS all physical interfaces interfaces have units (virtual interfaces).
reminder: Layer 3 configs always occurs at the unit level

**Vlan Tagging **

to configure a vlan:
_set vlan-tagging_
_set unit x vlan-id y_
_set unit X family inet address IP X_

