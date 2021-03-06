Title: BGP Failover 
Slug: bgp-failover
Date: 2013-11-21 16:03:24
Tags: cisco, bgp

BGP Failover 

BGP Failover is a common redundancy practice for most companies that demand a always on connection to they're services over the Internet. 

For these failover scenarios ill be using:

	- Multi-Exit Discriminator (MED - a.k.a Metric) - (non-transitive) - (Bigger is better) - default = o
	- Local Preference (Discretionary) - (Bigger is better) default = 100
	- Autonomous System Path (AS-PATH - Mandatory) - (Smaller is better)


Same ISP

Company A has a contract with an ISP and orders a second line for backup:

<img src="/media/ISP1.jpg" class="img-responsive">

To achieve redundancy they must

Announce main prefix on each link
The link that will be used as backup will increase  it's metric(MED) value for outbound traffic and local-preference for inbound traffic

Configuration example:
'''
router bgp [LOCAL_AS_NUMBER]

		network [NETWORK_ADDRESS] [NETMASK]

		neighbor [NEIGHBOR_IP_ADDRESS] remote-as [LOCAL_AS_NUMBER]

		neighbor [NEIGHBOR_IP_ADDRESS] prefix-list [LIST_NAME_OUT] out

		neighbor [NEIGHBOR_IP_ADDRESS] route-map [ROUTE_MAP_NAME_OUT] out

		neighbor [NEIGHBOR_IP_ADDRESS] prefix-list [LIST_NAME_IN] in

		neighbor [NEIGHBOR_IP_ADDRESS] route-map [ROUTE_MAP_NAME_IN] in

ip prefix-list [LIST_NAME_IN] [NETWORK/CIDR]

route-map [ROUTE_MAP_NAME_OUT] permit 10

	match ip address prefix-list [LIST_NAME_IN]

	set metric 10

route-map [ROUTE_MAP_NAME_OUT] permit 20

	route-map [ROUTE_MAP_NAME_IN] permit 10

	set local-preference 90			
'''



Different ISPs

Now that company A has achieved redundancy, they realize that if ISP1 has a problem both links will go down, killing all the work. So they kill the backup link from ISP1 and order one from ISP2:

<img src="/media/ISP2.jpg" class="img-responsive">


This time, the steps to achieve redundancy are a little different:

	- Again they announce main prefix on each link
	- But instead of using MED and Local Preference, the backup link increases AS PATH by using <i>AS PATH preprend</i>

Configuration example:
'''
router bgp [LOCAL_AS_NUMBER]

	network [NETWORK_ADDRESS] [NETMASK]

	neighbor [NEIGHBOR_IP_ADDRESS] remote-as [LOCAL_AS_NUMBER]

	neighbor [NEIGHBOR_IP_ADDRESS] prefix-list [LIST_NAME] out

	neighbor [NEIGHBOR_IP_ADDRESS] route-map [ROUTE_MAP_NAME] out

ip prefix-list [LIST_NAME] [NETWORK/CIDR]

route-map [ROUTE_MAP_NAME] permit 10

	set as-path prepend [LOCAL_AS_NUMBER] [LOCAL_AS_NUMBER] [LOCAL_AS_NUMBER]
