# Dual-Multihomed-WAN-ISP

## 🎯 Objective

This project simulates an enterprise network connecting to two Internet Service Providers (ISPs) using BGP. 

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
<img width="1853" height="963" alt="topology" src="https://github.com/user-attachments/assets/2f35b3c9-7114-4a63-acf6-5b4531c395fd" />

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
We use outbound here because we want routers on the ISP side to believe that it is a longer path to Edge-Router-02, influencing the path selection.  

### 1. NAT Translation Verification
Verify that the Gateway Router is actively translating private IP traffic to public-ready flows:
```text
Gateway Router(config)#do sh ip nat translations

Pro Inside global      Inside local       Outside local      Outside global
icmp 192.0.35.3:20032  192.168.10.1:20032 8.8.8.8:20032      8.8.8.8:20032

```

