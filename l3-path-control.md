Title: PBR & IP SLA
Author: Tiago Sousa
Date: 2011-07-29 12:28:34
Tags: CCNP, Certification, IP SLA, PBR, ROUTE


## Policy Based Routing


"match'em and set them on their way"

Remember:

Packets are **processed** by the routing table (or CEF) after being dissembled from their layer-2 frame.

Prior to being **routed** by the routing table (or CEF), **Policy-Based Routing **is applied


PBR is enforced with route maps




First, configure the **match** sequence

_match ip address_

_match length min max_

Then, redirect with **set** instructions:



	
  * _set ip next-hop __ip-address x.x.x.x (_ip must be on the network)

	
  * _set ip default next-hop _ip-address x.x.x.x (__Same as above, check note at the end_)_

	
  * _set interface_ _interface-type interface-number (_Forwards packet out selected interface)

	
  * _set default interface interface-type interface-number (Same as above, check note at the end_)__




Note that using the _**default**_ option, makes IOS to _first_ use the routing table, but if no route exists or if it fails, then use PBR





Finally, apply it to an interface:


_ip policy route-map route-map-name_

__To apply PBR to packets generated from router, use:

_ ip local policy route-map route-map-name_

To show interfaces where PBR is enabled

_show ip policy _

To view if PBR is bein matched/ statistics

_show ip route-map_




## IP Service Level Agreement




IP SLA´s are used to do simple monitoring, and, if needed, route trafic like PBR does

to monitor it can send



	
  * ICMP packets

	
  * TCP requests

	
  * HTTP GET packets

	
  * IP SLA responder (to sla enabled devices)

	
  * ...


SLA configuration guidelines:



	
      1. Create an operation with a number as an identifier




Define the operation type

Define frequency
Define when it will run

Configuration example:


_ip sla 1_
_ icmp-echo_
_ icmp-jitter icmp-echo 10.1.1.1 source-ip 10.1.1.2_
_ frequency 60_
_ ip sla schedule 1 start-time now life forever_




To verify  use: _show ip sla configuration_

__To see statistics of an SLA use: _show ip sla statistics_ _number of sla_


**Using IP SLA for routing**


A tracking object is associated with a IP SLA operation. Based on the IP SLA operation’s return code, the tracking object is able to determine an “up” or “down” state that can then be used by PBR to influence routing, and because the tracking object allows the addition of delay or tolerance, it works without causing route flaping

To configure:

_track_ _object-number_ ip sla _sla-number _[state|reachability]

_ip route destination mask [_interface_|_next-hop_] track _object-number

To configure a PBR with a tracking object, use the **verify-availability** parameter on the **set** command in the route-map:

_set ip next-hop verify-availability next-hop precedence track tracking-object_