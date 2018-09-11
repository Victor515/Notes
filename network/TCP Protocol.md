# TCP Protocol

## tcpdump

tcpdump for monitoring traffic for any protocol stack:

```bash
## Set up tcpdump
tcpdump -n host example.net 
## example reqeuest
printf 'HEAD / HTTP/1.1\r\nHost: example.net\r\n\r\n' | nc example.net 80
## example output
listening on wlp1s0, link-type EN10MB (Ethernet), capture size 262144 bytes
17:14:40.811222 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [S], seq 1837363917, win 29200, options [mss 1460,sackOK,TS val 1943389487 ecr 0,nop,wscale 7], length 0
17:14:40.820456 IP 93.184.216.34.80 > 10.126.177.128.47204: Flags [S.], seq 3867902848, ack 1837363918, win 65535, options [mss 1460,sackOK,TS val 425787226 ecr 1943389487,nop,wscale 9], length 0
17:14:40.820562 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [.], ack 1, win 229, options [nop,nop,TS val 1943389497 ecr 425787226], length 0
17:14:40.820702 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [P.], seq 1:39, ack 1, win 229, options [nop,nop,TS val 1943389497 ecr 425787226], length 38: HTTP: HEAD / HTTP/1.1
17:14:40.820770 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [F.], seq 39, ack 1, win 229, options [nop,nop,TS val 1943389497 ecr 425787226], length 0
17:14:40.829904 IP 93.184.216.34.80 > 10.126.177.128.47204: Flags [.], ack 39, win 283, options [nop,nop,TS val 425787228 ecr 1943389497], length 0
17:14:40.830558 IP 93.184.216.34.80 > 10.126.177.128.47204: Flags [P.], seq 1:344, ack 40, win 283, options [nop,nop,TS val 425787228 ecr 1943389497], length 343: HTTP: HTTP/1.1 200 OK
17:14:40.830603 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [.], ack 344, win 237, options [nop,nop,TS val 1943389507 ecr 425787228], length 0
17:14:40.830961 IP 93.184.216.34.80 > 10.126.177.128.47204: Flags [F.], seq 344, ack 40, win 283, options [nop,nop,TS val 425787228 ecr 1943389497], length 0
17:14:40.830987 IP 10.126.177.128.47204 > 93.184.216.34.80: Flags [.], ack 345, win 237, options [nop,nop,TS val 1943389507 ecr 425787228], length 0
^C
10 packets captured
14 packets received by filter
```



### Observation

* TCP data are sent through packets, some packets may have payloads, others may not.

* TCP packets have sequence number. And most OS will buffer TCP packet using sequence numbers. Sometimes data in different packets may be received in different order, or even overlapped with others. So we need to reorganize them.



##  TCP flags

There are 6 basic TCP flags

- SYN (synchronize) `[S]` — This packet is opening a new TCP session and contains a new initial sequence number.
- FIN (finish) `[F]` — This packet is used to close a TCP session normally. The sender is saying that they are finished sending, but they can still receive data from the other endpoint.
- PSH (push) `[P]` — This packet is the end of a chunk of application data, such as an HTTP request.
- RST (reset) `[R]` — This packet is a TCP error message; the sender has a problem and wants to reset (abandon) the session.
- ACK (acknowledge) `[.]` — This packet acknowledges that its sender has received data from the other endpoint. *Almost every packet except the first SYN will have the ACK flag set.*
- URG (urgent) `[U]` — This packet contains data that needs to be delivered to the application out-of-order. *Not used in HTTP or most other current applications.*



### Three-way handshake

The first packet sent to initiate a TCP session always has the SYN flag set. This *initial SYN packet* is what a client sends to a server to start opening a TCP connection. This is the first packet you see in the sample tcpdump data, with `Flags [S]`. This packet also contains a new, randomized sequence number (`seq` in tcpdump output).

If the server accepts the connection, it sends a packet back that has the SYN and ACK flags, and acknowledges the initial SYN. This is the second packet in the sample data, with `Flags [S.]`. This contains a different initial sequence number.

(If the server doesn't want to accept the connection, it may not send anything at all. Or it may send a packet with the RST flag.)

Finally, the client acknowledges receiving the SYN|ACK packet by sending an ACK packet of its own.

This exchange of three packets is usually called the **TCP three-way handshake**. In addition to sequence numbers, the two endpoints also exchange other information used to set up the connection.

### Four-way teardown

When either endpoint is done sending data into the connection, it can send a FIN packet to indicate that it is finished. The other endpoint will send an ACK to indicate that it has received the FIN.

In the example HTTP data, the client sends its FIN first, as soon as it is done sending the HTTP request. This is the first packet containing `Flags [F.]`.

Eventually the other endpoint will be done sending as well, and will send a FIN of its own. Then the first endpoint will send an ACK.

### In between

In a long-running connection, there will be *many* packets exchanged back and forth. Some of them will contain application data; others may be only acknowledgments with no data (`length 0`). However, all TCP packets in a connection except the initial SYN will contain an acknowledgment of all the data that the sender has received so far. Therefore, they will all have the ACK flag set. (This is why tcpdump depicts the ACK flag with just a dot: it's really common.)

### ICMP and UDP don't have TCP flags

If you look at tcpdump data for pings or basic DNS lookups, you will not see flags. This is because ping uses ICMP, and basic DNS lookups use UDP. These protocols do not have TCP flags or sequence numbers.





## TCP packet drop

Router may drop packets if there is traffic jam along the path of transmission(fast-slow network, for example

How does it work?

When there is congestion, the router will tell the sender to send slower by dropping packets. The sender will **detect** the loss, and will eventually send slower.

Example: 

``` bash
# Listen at port 12345
sudo tcpdump port 12345

# try to connect to www.udacity.com at port 12345
nc www.udacity.com 12345
```

Because udacity does not open port 12345, nsuackets sent will receive a ack back. Thus, all pakcets will be considered dropped. So the sender will send slower and slower, and eventually abandon the connection.