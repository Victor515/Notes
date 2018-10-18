# Big Networks



## Tools to test network connectivity and latency

traceroute and ping

round trip time

TTL(time to live)



## Bandwidth and latency measurement

bandwidth of networks



what is fast?

bandwidth and latency



bandwidth-delay product: how much data is in transit at any given time?



network congestion

how to deal with it? Senders should send more slowly



## MiddleBoxes

**Firewalls** are devices that network operators can use to filter traffic that's coming into or leaving their network.

A firewall is one example of a class of network devices called **middleboxes** — devices that inspect, modify, or *filter* network traffic. 

Technically, it's only a *middle*-box if it's a separate device from the client or server — server-side "firewalls" like Linux iptables aren't middleboxes.

Firewalls can cause trouble for application developers. One of the reasons that many non-Web applications use HTTP as a transport is that HTTP is often unblocked at firewalls even when other ports are blocked.

Aside from blocking traffic outright, middleboxes can also *alter* traffic, for instance replacing web pages with error messages. This is often done for social or political purposes.



With **NAT**, several devices can access Internet resources through a single public IP address, with the NAT device using port numbers to match up connections on the inside and outside.

For end-users, NAT devices overlap with firewalls. 

 At a larger scale, ISPs and other organizations have deployed NAT devices for their whole customer networks, called carrier-grade NAT. This is very common for mobile networks, and also for ISPs in the developing world.

In the case of NAT, your web site can see requests from the same IP address that actually come from different users on different computers.



Another way that can happen is through the use of **web proxies**. Whereas a NAT works at the IP level, rewriting packets, a web proxy works at the HTTP level, taking queries from browsers and sending them out to web servers.

From the standpoint of a web developer or site operator, traffic from a busy proxy looks much the same as traffic from a busy NAT: queries for many users, on many actual computers, are funneled through *a single public IP address*.

Question: How can a web application distinguishes users behind the same ip address?

**Logged-in user identity**

Speed of connections

**Session Cookies**

Ethernet or Wifi Mac Address

Geolocation