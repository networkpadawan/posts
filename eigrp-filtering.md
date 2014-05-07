Title: EIGRP Filtering
Author: Tiago Sousa
Date: 2011-07-19 13:01:46
Tags: CCNP, Certification, EIGRP, ROUTE


You can configure an EIGRP router to filter routes from being advertised or from being accepted.






To filter networks that the router sends EIGRP can use distribute lists with:



	
  * ACL

	
  * Prefix List

	
  * Route Map






## ACL



EGIRP distribute lists use standard named/numbered access lists
(remember the deny any any end statement)

to configure:
_access-list 1 deny 10.0.0.0 0.255.255.255_
_access-list 1 permit any_
_router eigrp 1_
_distribute-list 1 out | in_





## Prefix List






"Standard ACL´s on steroids!"

in the notation 192.168.1.0

the **network prefix** 192.168.1 and the **network prefix** length is /8 , so

the **network prefix** is the shared network statement in a network and  the** network prefix length** is the number of host that are part on that network

prefix list options:



	
  * ge - greater than or equal to

	
  * le - less then or equal to


to configure

_ip prefix-list plist1 deny 10.0.0.0/8_
_ ip preffix-list plist permit 0.0.0.0/0 le 6  ge 12_
_ router eigrp asn_
_ distribute-list prefix plist out_




## Route Map


"uses a if/then/else logic"

to configure:

_route-map tag1 permit|deny seq1_
_ match **statement**_
_ ****router eigrp asn_
_ distribute-list route-map 1 in|out_

statements can be:



	
  * acl

	
  * prefix list

	
  * protocol

	
  * metric


