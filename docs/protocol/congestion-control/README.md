# Congestion Control

Starting from v1.3.0, SRT provides two types of data transmission: live and file. The mode can be selected via the socket option `SRTO_CONGESTION` \(or more general `SRTO_TRANSTYPE` option\).

## Useful Links

* Yunhong Gu. [The UDT Congestion Control Algorithm](http://www.jenkinssoftware.com/raknet/manual/congestioncontrol.html). 2009.
* [Network Congestion Management: Considerations and Techniques](https://www.sandvine.com/hubfs/downloads/archive/whitepaper-network-congestion-management.pdf)
