# Dual-Multihomed-WAN-ISP

🎯## Objective

This project simulates an enterprise network connecting to two Internet Service Providers (ISPs) using BGP. 

* Configured eBGP and iBGP for dynamic routing & redundancy.
* Manipulated path selection via local preference and AS-path prepending.
* Built automatic fail-over mechanisms to maintain uptime during link or switch failures.
* Verified and tested network resilience and resource availability.

📖## Skills Learned

* Advanced eBGP and iBGP configuration
* Analyzing and interpreting verification commands
* Manipulating path selection (Local Preference, AS-Path Prepending)
* OSPF routing and `default-information originate always`
* Multi-homed Dual WAN setups
* Network troubleshooting

🛠️## Tools & Technologies Used

* Verification commands
* Prefix lists and route maps
* Bidirectional Forwarding Detection (BFD)

🗺️Topology Diagram

[Below is the structural layout of the simulated enterprise edge environment, showcasing dual multihomed wan/isp setup]
<img width="1853" height="963" alt="topology" src="https://github.com/user-attachments/assets/2f35b3c9-7114-4a63-acf6-5b4531c395fd" />

Device Configurations

Enterprise Edge (Primary): Edge-Router-01.txt — Handles primary internet outbound traffic. <br>
Enterprise Edge (Secondary): Edge-Router-02.txt — Acts as the secondary internet gateway. <br>
Internal Core Switch: Core-L3-Switch.txt — Manages LAN routing and HSRP gateways. <br>
Service Provider Gateway A: ISP-A-Router.txt — Simulates primary upstream ISP peering. <br>
Service Provider Gateway B: ISP-B-Router.txt — Simulates secondary backup ISP peering. <br>
