Title: Site to Site IPsec VPNs part 2
Slug: site-to-site-ipsec-vpns-part-2
Date: 2013-10-15 12:46:45
Tags: cisco, vpn, security, asa, ios

In the previous post, I set up a vpn between two ios routers, now let's try it between a ios router and a cisco ASA.
Last time there we're two ways two set up the vpn, either with a tunnel based vpn or a policy one. But this time we can only set up policy based vpns because ASAs don't support vti/gre tunnels.

<img src="/media/sts00.png" class="img-responsive">

I'll be using the same example as before, just replacing a router for the asa and instead of a loopback interface i'll add a connection to my laptop so I can access the ASDM


*Conf t* Time!

On the ios router please check the previous post.

On the ASA:

   <div class="bs-example bs-example-tabs">
      <ul id="myTab" class="nav nav-tabs">
        <li class="active"><a href="#cli" data-toggle="tab">cli</a></li>
        <li><a href="#asdm" data-toggle="tab">asdm</a></li>
          </ul>
        </li>
      </ul>
      <div id="myTabContent" class="tab-content">
        <div class="tab-pane fade in active" id="cli">

Set interfaces ip and nameif

```
interface gigabitethernet 0
ip address 172.16.1.1 255.255.255.0
nameif inside
interface gigabitethernet 1
ip address 172.16.2.1 255.255.255.0
nameif outside
```
Define isakmp policy
```
crypto isakmp policy 10
authentication pre-share
encryption aes
has sha
group 5
lifetime 86400
```
Enable isakmp on the interface
```
crypto isakmp enable OUTSIDE
```
Set the pre shared key
```
tunnel-group 172.16.2.2 type ipsec-l2l
tunnel-group 172.16.2.2 ipsec-attributes
pre-shared-key secretkey
```
Set the ACL to match the traffic that gets encrypted
```
access-list ALVPN permit ip 172.16.1.0 255.255.255.0 172.16.3.0 255.255.255.0
```
Define the transform set
```
crypto ipsec transform-set TS1 esp-aes esp-sha-hmac
```
Create the Crypto map
```
crypto map CM1 10 set peer 172.16.2.2
crypto map CM1 10 set transform-set TS1
crypto map CM1 10 match address ALVPN
crypto map MAP-OUTSIDE 20 set security-association lifetime kilobytes 10000
```
Apply the crypto map to the interface
```
crypto map CM1 interface OUTSIDE
```
Done!
```
ciscoasa# ping 172.16.3.1
Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.3.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 10/28/40 ms
```
</div>
        <div class="tab-pane fade" id="asdm">
	...assuming you have already the asa up and running and asdm access.
	<br />
	Let's use the wizard.
	<br />	
	<img src="/media/asasts1.png" class="img-responsive">
	<br />
	<img src="/media/asasts2.png" class="img-responsive">
	<br />
	<img src="/media/asasts3.png" class="img-responsive">
	<br />
	<img src="/media/asasts4.png" class="img-responsive">
	<br/>
	<img src="/media/asasts5.png" class="img-responsive">
	<br />
	If you're using nat, this option is helpful to restrict the vpn traffic from being *"nated"*.
	<br />
        <img src="/media/asasts6.png" class="img-responsive">
        <br />
        <img src="/media/asasts7.png" class="img-responsive">
        <br />
        <img src="/media/asasts8.png" class="img-responsive">
        <br />
	And a simple test to see if it's ok.
        <br />
        <img src="/media/asasts9.png" class="img-responsive">
        </div>
</div>

One "problem" that I had was initializing the tunnel. Traffic has to arrive at either site for negotiation to begin.

A goof tip is to use Packet Tracer to generate some packets to force the tunnel up.
