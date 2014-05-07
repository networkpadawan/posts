Title: BGP 101
Author: Tiago Sousa
Date: 2011-07-25 17:30:36
Tags: BGP, CCNP, Certification, ROUTE


BGP - _"Teh internet protocol"_

When to use?



	
  * Any multihomed scenario

	
  * Part of transit AS/Network

	
  * AS_Path info is needed


When _not to use?_



	
  * when network is singlehomed

	
  * not providing downstream routing





## faqs






Uses TCP - port 179

ebgp ad - 20

ibgp ad - 200

Updates incremental and triggered

Slow protocol

Very big Metric




the ASN is a 2 bit field



	
  * 0 - Reserved

	
  * [1-64495] - Assignable to public use

	
  * [64496-64511] - Reserved for use in documentation

	
  * [64512-65534] - Private AS´s

	
  * 65535 - Reserved




Auto-Summary  (same as eigrp) - is off by default on IOS ≥ 12.2(8)T. In earlier IOS, we have to deactivate it manually.





## Styles



default route only - Receive  just the default route

partial updates - receive routes that the isp is "well connected" plus a default route

Full update - Receive the entire  BGP table




## Packet Types





	
  * **Open** - starts session



	
  * **Keepalive** - used for dead neighbor detection (if hold = 0, no keepalives sent)



	
  * **Update**-  add or remove a prefix (exchanging prefix/lenght NLRIs and PAs)



	
  * **Notification **- Used to convey error messages, after notification is sent, the BGP session closes.





## Tables


Neighbor table - list of active peerings

BGP Table - All BGP routes

Routing Table - usual routing table, lists the best routes




## Peering


Requirements



	
  * bgp version (current default is 4)

	
  * local asn must match with the neighbor command on the other side

	
  * local router-id must be different

	
  * authentication (if enabled)

	
  * hold time - negotiated to lowest requested value

	
  * TCP connectivity

	
  * options - capabilities / attributes


Peering **State** machine



	
  * Idle - Waiting to start 3-Way HandShake

	
  * Connect - Waiting to complete 3-Way HandShake

	
  * Active - 3-Way HandShake failed, retrying

	
  * Open Sent -3-Way HandShake completed OPEN message sent

	
  * Open Confirm - OPEN message received, parameters agreed

	
  * Established - peering OK





## BGP Synchronization


"Do not advertise a route learned via IBGP until it is learned from the IGP"

As of 12.2(8)T, synchronization is disabled by default.

turn of with router command _no __synchronization_