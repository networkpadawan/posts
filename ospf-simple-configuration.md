Title: OSPF Simple Configuration
Author: Tiago Sousa
Date: 2011-07-07 16:25:49
Tags: CCNP, Certification, OSPF, ROUTE


Basic configurations:

**start OSPF routing:**

_router ospf process number_

_network x.x.x.x x.x.x.x area x_

_router-id x.x.x.x_

**To tweak ospf timers go to each interface and type**



	
  * _ip ospf hello-interval _

	
  * _ip ospf dead-interval_


**to convert an area into a stub area **

_router ospf process id_

_area X stub_ - stub area

**to convert the area into a totally stubby area**

on the ABR

_area x stub no-summary_

on the internal routers

_area x stub_

**to convert the area into a nssa area**

area x nssa [no-summary] | [no-redistribuition]

