Title: Route Redistribuition
Author: Tiago Sousa
Date: 2011-07-07 19:40:27
Tags: CCNP, Redistribuition, ROUTE


**Purpose?**

Redistribution is necessary when different routing protocols connect and must pass routes between them...




## Redistribution Issues


cost  - hop  - k values >> Different Metrics per protocol

AD´s

loops

seed metric



	
  * route redistribution

	
  * route filtering

	
  * BGP




## Resolving Redistribution Issues





	
  * **Passive Interface**




asdfsd




sasdf





	
  * **AD modification**




**
**





	
  * **Distribute Lists /Prefix Lists**






Distribute Lists

create access list

go to orouter process and applye access list

Distribute lists are access lists applied to the routing process, determining which networks are allowed into the routing table or included in

updates.  They essentially act as a filter.

[interface-name | routing-process | AS-number]



Think:  access list applied to routing = distribute lists



When creating a distribute list, use the following steps:

Step 1.

Identify the network addresses to be filtered and create an ACL – permitting the networks you want to be advertised.

Step 2.

Determine if you want to filter updates coming into the router or leaving the router.

Step 3.

Assign the ACL using the distribute-list command.

_distribute-list_ {acl-number | name} _in_| _out_ [interface-type number]  | [interface-name | routing-process | AS-number]






**
**





	
  * **Route Maps **




Route maps  are extremely flexible and are used in a number routing scenarios

including:

•  Controlling redistribution based on permit/deny statements

•  Defining policies in policy-based routing (PBR)

• Add more mature decision making to NAT decisions than simply

using static translations

• When implementing BGP PBR



Route maps allow an administrator to define specific traffic and then take

automated actions against it to control how routing information is processed and

forwarded.  Route maps uses logic similar to if/then statements in simple

scripting.

In route map terms, it matches traffic against conditions and sets options for

that traffic.

Each statement in a route map has a sequence number, which are read from lowest to highest.  The router stops reading statements when

it reaches its first matching statement.

Understand that there is an implicit deny included in all route maps.  If traffic does not match any statement, it is denied.









##########

Planning tip -_ __ _ -Only do Redistribution one-way..



seed metric - start metric...







Route Maps