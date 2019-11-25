# Bandwidth Estimation

SRT estimates the value of the link capacity. This is done by using a packet pair probing technique.

In this technique, the sender sends two back-to-back packets of the same size. Once these two packets arrive at the receiver side, there will be an interval between the two packets. The link capacity can then be determined by

B = packet size / interval

SRT sends out a packet pair every 16 packets. However, when it happens that there is no 17th packet to be sent out, SRT will still send out the 16th packet, rather than waiting for the next one. In this case, there will be a bigger interval \(hence underestimation\) at the receiver side. The receiver should use a median filter to detect and discard such values.

In addition, other patterns of packet pairs can be used, as long as the receiver has a way to identify the packet pairs.

* Ningning Hu, Peter Steenkiste. [Estimating Available Bandwidth Using Packet Pair Probing](http://www.cs.cmu.edu/afs/cs/project/cmcl/archive/papers/igi-tr.pdf)  . 2002.
* Yunhong Gu. [The UDT Congestion Control Algorithm](http://www.jenkinssoftware.com/raknet/manual/congestioncontrol.html). 2009.



