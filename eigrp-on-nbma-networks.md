Title: EIGRP & NBMA Networks
Author: Tiago Sousa
Date: 2011-07-16 19:35:38
Tags: CCNP, Certification, EIGRP, ROUTE


EIGRP can set manual neighbors but enabling manual disables automatic discovery

Frame relay can emulate point-2-point with the usage of subinterfaces, even so if only one dlci stays up the neighbor stays up until the hold timer expires.

Split Horizon



	
  * disabled on physical interfaces

	
  * enabled on sub interfaces


Bandwidth concerns

Because eigrp uses by default 50% of a link, it can easily oversubscribe a fractional T1/E1. Also EIGRP divides the interface evenly between the number of neighbors.

To correct go to the interface and manually set the bandwidth.

As a guideline use the value of the lowest CIR to the calculations or use a point-to-point design