Title: Site to Site IPsec VPNs part 1
Slug: site-to-site-ipsec-vpns-part-1
Date: 2013-10-10 17:46:45
Tags:cisco, vpn, securiy

There are two flavors of site to site vpn:

- *Policy based* - the traffic that matches a given policy will be encrypted.

- *Route based aka tunnel based* -  vpns encrypt any traffic that is directed through it.

For the sake of clarity let´s use this simple lab:


<img src="/media/sts00.png" class="img-responsive">

Using a vpn with crypto maps(policy vpn) we define that any traffic that is matched by a given policy will be encrypted and sent over the wire. It is very administrative heavy on the configuration, but provides security out of the box. 

<h4>How to configure Crypto Map VPNs</h4>

First, let's configure ips for serial and loopback on each interface

R1:
```
interface loopback 0
ip address 172.16.1.1 255.255.255.0
no shut
interface serial 1/0
ip address 172.16.2.1 255.255.255.0
no shut
```

R2:
```
interface loopback 0
ip address 172.16.3.1 255.255.255.0
no shut 
interface serial 1/0
ip address 172.16.2.2 255.255.255.0
no shut
```
Next, Pre Shared key Information

R1:
```
crypto keyring VPN
pre-shared-key address 172.16.2.2 key secretkey
```
R2:
```
crypto keyring VPN
pre-shared-key address 172.16.2.1 key secretkey
```
Next, the isakmp policy and profile :

R1 & R2:
```
crypto isakmp policy 10
encr aes 256
authentication pre-share
group 5
```
R1:
```
crypto isakmp profile R1R2
keyring VPN
match identity address 172.16.2.2 255.255.255.255 
```
R2:
```
crypto isakmp profile R2R1
keyring VPN
match identity address 172.16.2.1 255.255.255.255 
```

After those we need to define the transform-set and the ipsec profile

R1 & R2:
```
crypto ipsec transform-set TS esp-aes esp-sha-hmac

crypto ipsec profile ipsecVPN
 set transform-set TS
```

Next, with an acl, define what traffic gets encrypted:
R1:
```
ip access-list extended R1R2
permit ip 172.16.1.0 0.0.0.255 172.16.3.0 0.0.0.255
```

R2:
```
ip access-list extended R2R1
permit ip 172.16.3.0 0.0.0.255 172.16.1.0 0.0.0.255
```

Finnaly Set the crypto map to "glue" all these settings together:

R1:
```
crypto map CM1 10 ipsec-isakmp
match address R1R2
set peer 172.16.2.2
set transform-set TS
set isakmp-profile ipsecVPN
reverse-route static
```

R2:
```
crypto map CM1 10 ipsec-isakmp
match address R2R1
set peer 172.16.2.1
set transform-set TS
set isakmp-profile ipsecVPN
reverse-route static
```
And, apply it to the interface. 
R1 & R2:
```
interface serial1/0
crypto map CM1
```
In the end you'll have a new route point to the vpn peer
```
R1# show ip route

...output omited ...

S       172.16.3.0 [1/0] via 172.16.2.2
```
And a final test
```
R1# ping 172.16.3.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.3.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 8/19/24 ms
```

Easy ain't it? Now, onto the route/tunnel vpn.

![Alt Text](/media/sts01.png)

See any differences? :)
Yes, as can be seen a new segment of IPs is required when implementing this type of vpn.
In this scenario, instead of defining what traffic gets encrypted with an acl, one defines a route and points it to the peer address. Same purpose with a different solution basically. Example time.

<h4>How to configure Tunnel based VPNs</h4>

The configuration is equal to the policy based vpn, right until the crypto map configuration so i'll skip it.

After the ipsec transform set is defined, configure the vti tunnels:

R1
```
interface Tunnel0
 ip address 172.16.4.1 255.255.255.0
 tunnel source 172.16.2.1
 tunnel destination 172.16.2.2
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile ipsecVPN
```
R2
```
 ip address 172.16.4.2 255.255.255.0
 tunnel source 172.15.2.2
 tunnel destination 172.16.2.1
 tunnel mode ipsec ipv4
 tunnel protection ipsec profile ipsecVPN
```
As a test lets define a route for the R2 loopback on R1:
```
ip route 172.16.3.0 255.255.255.0 172.16.4.2
```
The result
```
R2# ping 172.16.3.1

Type escape sequence to abort.
Sending 5, 100-byte ICMP Echos to 172.16.3.1, timeout is 2 seconds:
!!!!!
Success rate is 100 percent (5/5), round-trip min/avg/max = 16/22/28 ms
```

This one is even easier to configure, (without the crypto map "glue") but has a catch that I'll tell you in the next article when I´ll add an ASA in to the mix.
