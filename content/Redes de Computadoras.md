# Bibliografia

Tanenbaum, A. S. Computer Networks. Prentice-Hall, 2003.

**Las sig. notas son de el libro**

# 1. Introduction

## 1.2 Netowrk Hardware

No hay una conjunto de categorías que clasifiquen perfectamente a las redes de computadora, pero si hay dos que son de mucha importancia: *transmission technology & scale*

### Classifying networks by transmission technology

Two major type: **broadcast** and **point-to-point** links

![[uni-and-broadcast.png]]

- **Point-to-Point**
	- Connect indivdual pairs of machines
	- To get to their destination, **packets** may have to visit intermediary machines, and there are many routes packets could take.
	- Point-to-Point networks where there is exactly one sender and one receiver can sometimes be called **unicast**
- **Broadcast**
	- The communication channel is shared by all machines, *packets sent by any machine, are received by all*
	- An **address field** within each packet specifies the intended recipient
	- When a machine receives a packet, if the address field matches (the packet was intended for the machine), it processes it, else it ignores it
	- Wireless networks are broadcast
	- As an analogy, consider someone standing in a meeting room and shouting ‘‘Watson, come here. I want you.’’ Although the packet may actually be received (heard) by many people, only Watson will respond; the others just ignore it.
	- Broadcast networks usually allow the possibility of addressing packets to *all* destinations, by using a special code in the address field, when this happens all machines in the network receive and process the packet, this is called **broadcasting**
	- Some networks support transmission to a *subset* of the machines, this is known as **multicasting**

### Classifying networks by scale

**Distance** is an important classification metric because *different technologies are used at different scales*

![[networks by scale.png]]

The connection of two or more networks is called an **internetwork**, the worldwide internet being the best example.

#### 1.2.1 Personal Area Networks

- **PANs** let devices communicate over the range of a person.
- **Bluetooth** is the best example of a PAN
- Bluetooth networks use the **master-slave paradigm**, the master tells the slaves what addresses to use, when they can broadcast, how long they can transmit, what frequencies they can use, and so on.

#### 1.2.2 Local Area Networks

- **Scale**: Operate within and nearby a single *building* (e.g. home, office)
- When LANs are used by companies, they are called **enterprise networks**
- **Modem**: Device that *converts digital signals into analog, and viceversa*

##### Wireles LANs

 - Very popular in homes, cafeterias, restaurants, etc.
 - In WLANs every computer has a radio modem and an antenna, each computer talks to an **Access Point (AP), wireless router, or base station**, these devices relay packets between the wireless computers, as well as between them and the internet.
 - WLANs follow the **IEEE 802.11**, also known as **WiFi**
![[wireless-vs-wired-lans.png]]

##### Wired LANs

- Use copper wires or optical fiber
- Compared to wireless networks, wired LANs have higher speeds (up to 1 Gbps, and some up to 10 Gbps), low delay and make very few errors, they *exceed them in all dimensions of performance* 
- The most common topology for wired LANs is *point-to-point links*, they follow the **IEEE 802.3** standard, commonly called **Ethernet**
- In **Switched Ethernet** every computer speaks the Ethernet protocol and connects to a box called a **switch** by a point-to-point link. A switch has multiple **ports** which connect computer to the switch. The switch functions as a **relay** for packets between computers that are attached to it

##### Virtual LANs

- It is possible to divide one large *physical* LAN into two smaller *logical* LANs, and it is useful when the network layout does not match the organization's structure
- To do this, **each port is tagged with a "color"**, say green for VLAN1 and red for VLAN2. The switch then forwards packets so that computers attached to ports of one color are separated from the computers attached to ports of another color
- This way, we can *broadcast packets to certain VLANS*

##### Static and Dynamic Allocation

- There are other topologies for wired LANs, *switched Ethernet is a modernized version of the original Ethernet*, which we will call **classic Ethernet**
- In classic Ethernet all packets are broadcasted in a single linear cable, *at most one machine could successfuly transmit at a time*, because of this a mechanism was needed to resolve conflicts. The mechanism was simple, computers could only transmit whenever the cable was idle, if two or more packets collided, each computer waited a random amount of time and tried again.
- *B*