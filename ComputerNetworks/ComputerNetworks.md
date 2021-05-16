## Computer Networks (NPTEL)
---
**TCP/IP Protocol Stack**
![Data Link,Network Layer use](p0.png)
- Concentrator/Hub/Repeater - Multiport thing. They can also act as an amplifier. 
- Media access protocol ~ lot of collision/loss B/W.
- Data Link ~ (L2) ~ Reduce collision.
- Network Layer ~ Routing. (Differnt Ports) [For Bigger Network to Network Connection]
- Transport Layer ~ Process to Process communication is handled by this layer. 
- Application Layer ~
![Pstack](p1.png)

Different Protocols at each layer.
1. Application Layer -> HTTP,FTP,SMTP
2. Transport -> TCP,UDP,RTP,ICMP
3. Network -> IPv4,IPv6,MPLS
4. Data Link -> Ethernet,WiFi,Bluetooth,UMTS,LTE
5. Physical -> 
6. Cross layer Protocols -> (DNS App/Tran),(ARP,DHCp App/Tran),SNMP.

**OSI(Open System Interconnection) Layer**
- Application `network application such as file transfer and terminal emulation.`
- Presentation `data formatting and encryption.`
- Session `establishment and maintenance of sessions.`
- Transport `end to end reliable and unreliable delivery.`
- Network `delivery of packets of information, which includes routing.`
- Data Link `transfers unit of information, framing and error checking.`
- Physical `transmission of binary data of a medium.`

> Unlinke LAN, a WAN connection is generally rented out from service providers.
> NICs provide hosts with access to media using a MAC address.
> MAC stands for Media Access Control
> NICs operate at Layer 2. 

- Hub share BW b/w all devices. If it is 10 Mbps and 8 devices  ~ 10/8 Nbps.
Hubs are stupid L1 devices, they cannot filter traffic. 

![HUB](p2.png)

- Most LANs use broadcast topology so every device sees every packet sent down the media. 
But bridges filter traffic based on MAC addresses.(smarter than hub)

![Bridge](p3.png)

- A switch(also known as multi-port bridge) can replace these four bridges.
Another benefit is that each LAN segment gets dedicated BW.

![Switch](p4.png)

- Routers filter traffic based on IP address. The IP address tells the router which LAN segment the ping belongs to.

**CORE/DISTRIBUTiON/ACCESS -**

**Broadcast domain - set of all devices on a network segment that hear all the broadcasts sent on that segment.**

**Collision domain - used to describe a network scenario in which on paritcular device sends a packet on a network segment, forcing other divices on the same segemtn to pay attention to it. At the same time, a different device tries to transmit, leading to ccollision then both the divices must re-transmit - a situation found in a Hub.**

**Switches break up collision domains. Each and every port on a switch represent its own collision domain ( Hub represents only one collision domain and only one broadcast domain )**

`Switching nodes donot concern itself with the content of data. The purpose is to provide a switching facility that will communicate/transmit the data from source to destination via itermediate node(s).`
![Switching](p5.png)
**Circuit Switching** 
---

**Packet Switching**
---




## MODULE 2 ( Application Layer )
---
1. DNS - DNS severs translate domain names to IP addresses. 
Eg:- com,org,net,gov,mil.... [TDLs or top level domains.]
![Domain](p6.png)