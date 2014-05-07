Title: JunOS Routing
Author: Tiago Sousa
Date: 2011-06-08 16:35:53
Tags: JNCIA, Juniper


Routing table

to display type show route

junos supports multiple routing tables

inet.0 display IPv4 destination routes

directed connected subnets show as** direct routes**

TSHOOT

to show hidden routes and check why they're hidden type
show hidden routes | exclusive

to see detail of a single route use the

show route _route _exact [detail|extensive] command

show route receive-protocol > shows the external received routes


### Route Preference Values










Source


Default Preference






Direct


0






Local


0






Static


5






OSPF Internal


10






RIP


100






Aggregate


130






OSPF AS External


150






BGP


170






## Static Routes


To configure static routes go to:

_edit routing-options _

and add_
_

_set static route 2.2.2.2/24 next-hop se-1/0/0.0 _

as for a default route:

_set static route default next-hop 1.1.1.1 _

_define several next hop with _

_edit static route 2.2.2.2/24 _and then_
_

set qualified next-hop 1.1.1.1 interface ge-0/0/0.0 preference X






## RIP




## OSPF







## BGP