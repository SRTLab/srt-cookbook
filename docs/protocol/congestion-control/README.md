# Congestion Control

Starting from v1.3.0, SRT provides two types of data transmission: live and file. The mode can be selected via the socket option `SRTO_CONGESTION` \(or more general `SRTO_TRANSTYPE` option\).

Live congestion control is used in live transmission mode. In this more the source has a certain rate (either constant or variable). Packets should be transmitted with the same rate as the source. Therefore, the congestion control module does not try to avoid network congestion. It is mainly a reactor on network events. Based on the network conditions (mainly RTT), LiveCC determines when a packet should be considered lost. On the receiving side, liveCC may decide when an ACK is needed prior to ACK timeout. LiveCC also determines the minimum time between consecutive packets are sent.

File congestion control is used in file transmission mode. FileCC controls sending pace to avoid network congestion.



 the only job congestion control module does is to keep sending rate.

## Useful Links

* Yunhong Gu. [The UDT Congestion Control Algorithm](http://www.jenkinssoftware.com/raknet/manual/congestioncontrol.html). 2009.
* [Network Congestion Management: Considerations and Techniques](https://www.sandvine.com/hubfs/downloads/archive/whitepaper-network-congestion-management.pdf)
