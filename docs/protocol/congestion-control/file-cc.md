# File Congestion Control

The main goal of File congestion control is to send data at the highest possible rate within the given network conditions. Sending with higher rate than the avalable bandwidth of a link will result in  network congestion, forcing huge overhead due to necessity of packet retransmission. Sending with the rate lower, that the available bandwidth, results in underutilization of the network link.

FileCC has three operation states: slow start, congestion avoidance and slow down.

At the start, there is no information on the network. Congestion control module has to do some probing to determine the available bandwidth and the best sending pace for the normal operation mode - congestion avoidance.

In congestion avoidance state, as long as there is no congestion detected (via Loss reports), the sending pace is gradually increased. In case of network congestion, the state is switched to slow down, where the sending pace is reduced to eliminate packet loss in a congested network.

## Slow Start

Slow-start state has an artificial limit on the number of data packets that can be sent before acknowledgment of those segments is received. Slow-start is designed to limit network congestion [1].

File CC starts with 16 DATA packets (`CWND_SIZE=16`) sent with minimum possible interval.

```c++
int m_iRCInterval;          // SRT Rate control interval
```

### Upon ACK Received

!!! Note "Only Full ACK triggers"

    Lite ACK do not trigger this case!

ACK are processed not more often than every 10 milliseconds (`CUDT::COMM_SYN_INTERVAL_US`).

`CWND_SIZE` is increased to the difference between the acknowledged sequence number since last ACK processing, and the sequence number being acknowledged:  `CWND_SIZE += LAST_ACK_SEQNO - ACK_SEQNO`.

If `CWND_SIZE` exceeds `MAX_CWND_SIZE`, slow start period ends. `MAX_CWND_SIZE=FLOW_WND_SIZE`. If delivery rate was reported by the receiver, the inter sending interval is set according to that rate.

```
PKT_SND_PERIOD = 1000000.0 / DELIVERY_RATE
```

where `DELIVERY_RATE` is filtered with `IIR(8)`.

If delivery rate is not reported, the sending period is set to be

```
PktSndPeriod = CWndSize / (RTT + RCInterval)
```

## Upon timers are checked

Refer to `FileCC::speedupToWindowSize(...)`.

When `CUDT::checkRexmitTimer(...)` is called and there is a timeout for the last response from the receiver, then slow start period ends, and the packet send period is set according to the above rules.

## Congestion Avoidance

### Upon ACK

### Upon Loss Report (NACK)

## References

1. TCP/IP Characteristics. Windows Dev Center. [[WEB](https://docs.microsoft.com/en-us/windows/win32/winsock/tcp-ip-characteristics-2)]