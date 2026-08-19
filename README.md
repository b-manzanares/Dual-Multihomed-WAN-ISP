# Dual-Multihomed-WAN-ISP

🚀## Objective

This project simulates an enterprise network connecting to two Internet Service Providers (ISPs) using BGP. 

* Configured eBGP and iBGP for dynamic routing & redundancy.
* Manipulated path selection via local preference and AS-path prepending.
* Built automatic fail-over mechanisms to maintain uptime during link or switch failures.
* Verified and tested network resilience and resource availability.

## Skills Learned

* Advanced eBGP and iBGP configuration
* Analyzing and interpreting verification commands
* Manipulating path selection (Local Preference, AS-Path Prepending)
* OSPF routing and `default-information originate always`
* Multi-homed Dual WAN setups
* Network troubleshooting

## Tools & Technologies Used

* Verification commands
* Prefix lists and route maps
* Bidirectional Forwarding Detection (BFD)

Topology Diagram

[Below is the structural layout of the simulated enterprise edge environment, showcasing dual connections to ISP-A and ISP-B.(Note: Replace this with your actual image file name once uploaded to your repository)]

Device Configurations

Enterprise Edge (Primary): Edge-Router-01.txt — Handles primary internet outbound traffic. <br>
Enterprise Edge (Secondary): Edge-Router-02.txt — Acts as the secondary internet gateway. <br>
Internal Core Switch: Core-L3-Switch.txt — Manages LAN routing and HSRP gateways. <br>
Service Provider Gateway A: ISP-A-Router.txt — Simulates primary upstream ISP peering. <br>
Service Provider Gateway B: ISP-B-Router.txt — Simulates secondary backup ISP peering. <br>
