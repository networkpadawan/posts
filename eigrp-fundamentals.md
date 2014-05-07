Title: EIGRP Fundamentals
Author: Tiago Sousa
Date: 2011-07-16 15:09:03
Tags: CCNP, Certification, EIGRP, ROUTE


In a nutshell... EIGRP



	
  * Has Backups routes

	
  * Supports Unequal cost Load-balancing

	
  *  Is easy to configure

	
  * Can summarize in any link on the topology

	
  * Uses IP protocol 88 and  Multicast address 224.0.0.10 via RTP





## EIGRP Tables


Neighbor table - directly connected devices

Topology table - Lists best networks (Successor ) and Backup Networks (Feasible successor)

Routing table - "_Routing table"_




## EIGRP Terms






	
  * Feasible Distance - **FD** - how far the route is from the router to destination

	
  * Advertised Distance - **AD** - How far the network is from the neighbor

	
  * Successor - Primary route

	
  * Feasible Sucessor - "Backup route" ( To be considered a Feasible Successor, the **AD** must be less than the **FD** of the successor)

	
  * Feasibility Condition - "if your metric is lower than mine, then you´re loop free"

	
  * Active Route - no route satisfies the feasible condition

	
  * Passive Route -  successor identified and ok





## EIGRP Neighborhood and packets






	
  * Hello packet (forms and maintains the neighborhood):

	
    * ASN

	
    * Hold time

	
    * Auth

	
    * K values




	
  * Update packet


	
    * Prefix + Lenght

	
    * Next hops

	
    * K values

	
    * MTU

	
    * Hop count

	
    * External Atributes


	
  * Query - look for route

	
  * Reply -  response to query

	
  * Ack - acknowledges all packets except hello


When the adjacency is formed full updates are sent with all the topology, after that only partial updates are sent when
- metric for a route changes
- new route added
Neighbors send hello packets every 5 sec on broadcast / point2point/ > T1 multipoint circuits and 60 sec on < T1 multipoints. the hold time is 3 times the above and a 3:1 ratio is advised when tweaking hello/hold timers.

Neighbor Requirements:

	
  * Same K Values

	
  * Same subnet

	
  * Interface not on passive-interface state

	
  * Same ASN

	
  * Authentication Key (if present)





## EIGRP Metric


metric variables:



	
  * K1 - Bandwidth - default value is **1**

	
  * K2 Loading - default value is** 0**

	
  * K3 - Delay - default value is **1**

	
  * K4  / K5 - Reliability - default value is **0**




The textbook metric is as follows:


##### K1*B1+((K2*BW)/256-load)+K3*delay)*(K5/reliabilty+K4)




but the real one used(see variable defaults) is:


#### 256*(BW + all link delays)




BW =10^7/Slowest_Link


delay in microsseconds




## Authentication


only supports md5

Example configuration:

> configure the keychain

    
    key chain <em>KEYNAME</em>



    
     key 1
      key-string cisco1
      accept-lifetime 00:00:00 Jan 1 2010 00:00:00 Feb 1 2010
      send-lifetime 00:00:00 Jan 1 2010 00:00:00 Feb 1 2010
     key 2
      key-string cisco2
      accept-lifetime 00:00:00 Jan 28 2010 infinite
      send-lifetime 00:00:00 Jan 28 2010 infinite


> go to the interface to enable and set up the keys 

    
    <em>interface X/X</em>



    
    <em>ip authentication mode eigrp ASN md5</em>



    
    <em>ip authentication key-chain eigrp ASN KEYNAME</em>








## EIGRP Stubs





	
  * Connected (Default) - advertises routes that are configured or redistributed

	
  * Summary (Default) - advertises summary routes both learned and configured

	
  * Static - advertises static routes but must be redistributed into eigrp first

	
  * Receive-only - no network advertisement

	
  * Redistributed - advertises routes redistributed from other AS or protocol

