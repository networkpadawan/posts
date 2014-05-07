Title: JunOS Firewall and COS 
Author: Tiago Sousa
Date: 2011-06-08 16:36:42
Tags: JNCIA, Juniper


## Firewall Filters


ntp 128 miliseconds

* Define a term
* Explain when import and export policies are evaluated in relation to the learning and advertising of prefixes
* List several match criteria and actions for firewall filters and routing policy
* Explain some common match criteria and
* Identify the results of a route or packet with a given filter or policy
* Configure a routing policy and firewall filters

Term -

JunOS firewall filters function almost like Cisco IOS ACL´s

the end term filter can either **discard **(drop the packet without warning) or **reject **(drop the packet and send a user-configurable error message)

Unlike IOS, JunOS uses standard network masks notation

Firewall Filters are compiled and on some router plataforms (T,M and MX Series) the firewall filters are processed as part of the route lookup process

To make changes to a current Firewall Filter, edit the candidate configuration, and by doing so obtain minimal disruption to secu

To configure

configure the firewall filter

_edit firewall family inet filter __example _and add rules_
_

add the filter to a interface and choose direction

_set unit 0 family inet filter input/output example ?_


## COS