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

**PANs**