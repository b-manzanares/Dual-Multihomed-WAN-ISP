# Dual-Multihomed-WAN-ISP

## 🎯 Objective

This project simulates an enterprise network connecting to two Internet Service Providers (ISPs) using BGP. The question I was trying to answer was "how would you create a redundant network, that provides access to the internet even if an ISP goes down?"

* Configured eBGP and iBGP for dynamic routing & redundancy.
* Manipulated path selection via local preference and AS-path prepending.
* Built automatic fail-over mechanisms to maintain uptime during link or switch failures.
* Verified and tested network resilience and resource availability.

## 📖 Skills Learned

* Advanced eBGP and iBGP configuration
* Analyzing and interpreting verification commands
* Manipulating path selection (Local Preference, AS-Path Prepending)
* OSPF routing and `default-information originate always`
* Multi-homed Dual WAN setups
* Network troubleshooting

## 🛠️ Tools & Technologies Used

* Verification commands
* Prefix lists and route maps
* Bidirectional Forwarding Detection (BFD)

## 🗺️ Topology Diagram

[Below is the structural layout of the simulated enterprise edge environment, showcasing dual multihomed wan/isp setup]

<img width="1853" height="963" alt="topology" src="https://github.com/user-attachments/assets/62bc73e5-79e3-4771-826f-765e909d55ab" />


## Device Configurations

Enterprise Edge (Primary): Edge-Router-01.txt — Handles primary internet outbound traffic. <br>
Enterprise Edge (Secondary): Edge-Router-02.txt — Acts as the secondary internet gateway. <br>
Internal Core Switch: Core-L3-Switch.txt — Manages LAN routing and HSRP gateways. <br>
Service Provider Gateway A: ISP-A-Router.txt — Simulates primary upstream ISP peering. <br>
Service Provider Gateway B: ISP-B-Router.txt — Simulates secondary backup ISP peering. <br>

## ☑️Configuration & Verification Snippets

### 1. Inbound Path Control via AS-Path Prepending

To prevent ISP-B from being used for inbound corporate traffic during normal operations, the secondary edge router prepends its Autonomous System (AS) number three times to advertisements sent to ISP-B. We might wish to alter how traffic arrives from the internet, since an ISP could cost less or if we have stateful connections the traffic could be blocked on the return trip if the firewall hasn't seen an already established connection. 

```text
! Configuration on edge-router-02

route-map AS_prepend permit 20
 set as-path prepend 65000 65000 65000
!
router bgp 65000
 neighbor 173.58.16.162 route-map PREPEND-ISP-B out
```
We use outbound at the end of the route-map because we want routers on the ISP side to believe that it is a longer path to Edge-Router-02, influencing the path selection. Here is the result ISP-A is preferred.  

```text
inserthostname-here(config)#do sh bgp

     Network          Next Hop            Metric LocPrf Weight Path
 *>   1.1.1.1/32       175.0.35.253                           0 650002 65000 i
 *>   2.2.2.2/32       175.0.35.253                           0 650002 65000 i
```
Now if we shutdown the link to ISP-A our route to ISP-B takes over and shows ups the Path. Here is the output 

```diff
inserthostname-here(config)#do sh bgp

     Network          Next Hop            Metric LocPrf Weight Path
+ *>   1.1.1.1/32       158.0.35.161                           0 650001 65000 65000 65000 65000 i
+ *>   2.2.2.2/32       158.0.35.161                           0 650001 65000 65000 65000 65000 i
```
### 2. Verification of BGP Neighbor States

Running show ip bgp summary confirms that peerings are properly established (State/PfxRcd shows a numerical value of prefixes received rather than an active state like Active or Idle).

inserthostname-here(config)#do sh ip bgp sum

Neighbor        V           AS MsgRcvd MsgSent   TblVer  InQ OutQ Up/Down  State/PfxRcd
158.0.35.161    4       650001      64      62       32    0    0 00:48:28        8
175.0.35.253    4       650002      19      19       32    0    0 00:06:40       13

### 3. BFD Session Verification

Verify sub-second path tracking state across the shared Layer 2 switch network. We see here that the Holdown is in miliseconds, allowing for faster convergence and almost instant fail-over. Sometimes this is needed in the case of a switch connecting two routers. Unless the routers are directly connected they will not know that others links went down until OSPF and BGP dead timers hit 0, which can be a long time by default. 

```diff

NeighAddr                              LD/RD         RH/RS     State     Int
200.0.2.254                             1/1          Up        Up        Gi0/1
+Session state is UP and using echo function with 300 ms interval.
Session Host: Software
OurAddr: 200.0.2.253
Handle: 1
Local Diag: 0, Demand mode: 0, Poll bit: 0
+MinTxInt: 1000000, MinRxInt: 1000000, Multiplier: 3

``` 

### 1. NAT Translation Verification
Verify that the Gateway Router is actively translating private IP traffic to public-ready flows:
```text
Gateway Router(config)#do sh ip nat translations

Pro Inside global      Inside local       Outside local      Outside global
icmp 192.0.35.3:20032  192.168.10.1:20032 8.8.8.8:20032      8.8.8.8:20032

```

