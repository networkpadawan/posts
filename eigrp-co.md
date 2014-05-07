Title: EIGRP Configuration and Troubleshoot
Author: Tiago Sousa
Date: 2011-07-10 16:14:09
Tags: CCNP, Certification, EIGRP, ROUTE


## Generic Configuration


_router eigrp process id_

_network x.x.x.x y.y.y.y  |_ match any interface with the address ...


## Tweak Hello / Hold / Active Timer





	
  * change hello interval:


_interface X/X_


_ip hello-interval eigrp asn seconds_





	
  * change hold time:


_interface X/X_

_ip hold-time eigrp asn seconds_






	
  * modify the active time of the router:





_timers  active timer _


## Default Route


_ip default-network x.x.x.x_

_ip route 0.0.0.0 0.0.0.0 x.x.x.x_

_router eigrp asn_

_network 0.0.0.0_


## Passive interfaces


disables the sending of hello|updates packets

_router eigrp asn_

_passive-interface | default_


## Summarization


_no auto-summary_

_ip summary-address eigrp asn x.x.x.x y.y.y.y_


## Unequal load balancing


_router eigrp asn_

_variance 1|128_


## Disable Split Horizon


_interface X/X_

_no ip split-horizon eigrp ASN_


## Tweak Bandwidth


_interface X/X_

_ip bandwidth-percent eirgp ASN mai_






# Troubleshooting EIGRP


Useful commands:






	
  * _**show ip eigrp interfaces**_ - Lists the working interfaces on which EIGRP is enabled (based on the network commands) omits passive interfaces.

	
  * _**show ip protocols**_ -Lists the contents of the network configuration commands for each routing process, and a list of neighbor IP addresses.

	
  * _**show ip eigrp neighbors**_ - Lists known neighbors; does not list neighbors for which some mismatched parameter is preventing a valid EIGRP  neighbor relationship.

	
  * _**show ip eigrp topology**_ - Lists all successor and feasible successor routes known to this router.

	
  * _**show ip eigrp topology all-links**_ - same as the above but shows also shows all links in the topology table

	
  * _**show ip route**_ - Lists the contents of the IP routing table










### Stuck in active


One of the most common issues with EIGRP is SIA. Caused by bad designs or unforeseen scalability needs, causes a router to wait a long time for a query to a network.

Newer version of IOS also have the ability to send SIA-Queries/replies, while waiting for a acknowledgement (default = 3 minutes) to check if the neighbor is still up and processing EIGRP.

It can be mitigated with a stub networks and summarization.



**Graceful shutdown** - sends a hello packet with different K values so the neighborhood goes down, older version of IOS might detect it has a "k value mismatch"