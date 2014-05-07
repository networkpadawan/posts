Title: OSPF Standard Features
Author: Tiago Sousa
Date: 2011-07-06 22:36:22
Tags: CCNP, Certification, OSPF, ROUTE





## Virtual Links







Because all areas must be connected to area 0, this duck tape-like feature was implemented.




Use in the case of  discontiguous area 0 or a new area that cannot be directly connected to area 0.




Both end routers must share a common area(and cannot be any kind of stub area) and at least one of the interfaces must directly connect to Area 0




Configure by issuing command area _area-number virtual-link router-id_ on each of the transit ABR´s




Verify that  virtual link is up with the _show ip ospf virtual-links_ command





## OSPF Summarization




With OSPF one can filter and summarize in 2 places






	
  * **In the ABR with Type 3 LSA´s**




Inter area summarizations are created on the ABR using the _area X range x.x.x.x x.x.x.x  [advertise | not advertise] cost x _command





	
  * **In the ASBR with Type 5 LSL´s**




External area summarizations  are created on the ASBR using the summary-address x.x.x.x x.x.x.x _ [not advertise] _ command







Type 7 LSA´s are used on the ABR with a NSSA area, when it has external information that has to be propagated




on the ABR issue  the area command _area area x nssa [no-summary] _





## OSPF Authentication


The Open Shortest Path First (OSPF) protocols has a provision for authentication, and the type of authentication can be indicated by a code number:

0 - no auth
1 - clear text auth
2 - md5 auth

To configure clear text authentication



	
  * ip ospf authentication-key KEY

	
  * ip ospf authentication

	
  * area x virtual-link x.x.x.x authentication-key KEY


To configure md5 authentication:






	
  * ip ospf message-digest-key KEYNUMBER md5 KEY

	
  * ip ospf authentication message-digest

	
  * area x virtual-link x.x.x.x  message-digest-key KEYNUMBER md5 KEY


