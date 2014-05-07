Title: BGP Attributes
Author: Tiago Sousa
Date: 2011-07-28 16:10:45
Tags: BGP, CCNP, Certification, ROUTE


Attributes can be** Well-Known **





	
  * Mandatory - Required fields by all BGP participants

	
  * Discretionary - not mandatory, but if applied, all BGP speakers will apply the attribute




Or,** Optional**





	
  * Transitive - BGP speaker might not recognize it, but it marks the update as partial and sends the update

	
  * Non Transitive - Non-transitive attributes are dropped if they fall onto a router that does not understand or recognize the attribute.






### **Well Known Attributes**





	
  * Autonomous System Path (AS-PATH - Mandatory)

	
  * Next Hop Address (mandatory)

	
  * Origin (mandatory)

	
  * Local Preference (Discretionary)

	
  * Atomic Aggregate (Discretionary) (summary routes)





### **Optional Attributes**





	
  * Aggregator - (transitive)

	
  * Multi-Exit Discriminator (MED - a.k.a Metric) - (non-transitive)

	
  * Weight - (Cisco Proprietary)

	
  *  Cluster ID - (non-transitive)

	
  * Originator ID - (non-transitive) - created by the route reflector. The attribute contains the router ID of the router that originated the route in the update. The purpose of theis attribute is to prevent a routing loop. If the originating router recieves its own update, it ignores it.

	
  * Community - (transitive)





## BGP Best Path Decision Order





**"We Love Oranges AS Oranges Mean Pure Refreshment"**









	
  1. Ignore routes with inaccessible next-hop address

	
  2. **Weight** (Bigger is better) - default = **o**

	
  3. **Local preference** (Bigger is better) default = **100**

	
  4. **Originated Locally **(Locally injected is better than iBGP/eBGP learned)

	
  5. **AS-Path** (Smaller is better)

	
  6. **Origin** (Prefer ORIGIN code I over E, and E over ?)

	
  7. **MED** (Smaller is better) default = **0**

	
  8. **Path** (Prefer eBGP path over iBGP path)

	
  9. **IGP cost** (Smaller is better)

	
  10. **EBGP Peering** (Older is better)

	
  11. **RID** (Lower is better)





## Attribute Influence





	
  * Weight influences the router where weight is added

	
  * Local Preference influences the AS where

	
  * MED influences the other AS´s to where the routes are sent


