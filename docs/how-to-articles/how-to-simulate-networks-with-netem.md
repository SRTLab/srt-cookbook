# Using NetEm to emulate networks

[NetEm](https://www.linux.org/docs/man8/tc-netem.html) (Network Emulator) is an enhancement of the Linux traffic control facilities that allow to add delay, packet loss, duplication and more other characteristics to packets outgoing from a selected  network  interface.  NetEm  is  built using  the  existing  Quality  Of Service (QOS) and Differentiated Services (diffserv) facilities in the Linux kernel.

It's a great tool to test SRT connections with network impairments like on real networks.

## Installation 

The Linux traffic control (tc) and NetEm (netem) are available in most linux distributions and typically part of the `iproute2` package.

On Ubuntu systems can be installed by following command:

```
sudo apt-get install iproute2
```

## Installation on CentOS 8.2

On CentOS 8 it's part of `iproute-tc`

```
sudo yum install iproute-tc
```

RHEL based systems like CentOS 8.2 might need an additional Kernel module, if the look-up of the name for `qdisc` fails, like in example shown below:

```
sudo tc qdisc add dev enp7s0 root netem delay 10ms 10ms loss 2% rate 12mbit
Error: Specified qdisc not found.
```

The modules `kernel-modules-extra` and `kernel-debug-modules-extra` can be installed with following command:

```
sudo yum install kernel-debug-modules-extra kernel-modules-extra
```

After installation a reboot is needed to activate the kernel modules.

## Getting started with tc & netem

Following examples use the interface `enp7s0`. Modify to match your network adapter (e.g. `eth0`).

To be able to change settings, root privileges are required.

### add delay

Please note: netem only adds delay to packets leaving the interface. If you want to simulate bi-directional delay two instances of tc netem - one on each end - are required. 

following command adds 250 ms of delay to packets leaving interface `enp7s0`:

```
sudo tc qdisc add dev enp7s0 root netem delay 250ms
```

### add delay with volatility

Following command sets the transmission of enp7s0 network interface to be delayed by 100ms + 10ms (any value between 90 and 110 ms)

```
sudp tc qdisc add dev enp7s0 root netem delay 100ms 10ms
```

The randomness of such fluctuations can be further specified:

Following command sets network interface enp7s0 transmission to 100 ms, while about 30% of the packets are delayed by +10 ms.

```
sudo tc qdisc add dev enp7s0 root netem delay 100ms 10ms 30%
```

### adding packet loss

Following command sets the enp7s0 network interface transmission to randomly drop 1% of the packets.

```
sudo tc qdisc add dev enp7s0 root netem loss 1%
```

### adding packet loss with success rate

```
sudo tc qdisc add dev enp7s0 root netem loss 1% 30%
```

### duplicating packets

```
sudo tc qdisc add dev enp7s0 root netem duplicate 1%
```

### corrupted packets

When Linux kernel version is above 2.6.16 packets can also be damaged. Following command randomly damages 0.2% of the outgoing packets:

```
sudo tc qdisc add dev enp7s0 root netem corrupt 0.2%
```

### packet reordering

Following command sets network interface enp7s0 transmission to sent out 25% Data package (with 50% relevance) immediately and delay all other packets by 10 ms.

```
sudo tc qdisc add dev enp7s0 root netem delay 10ms reorder 25% 50%
```

### view the configured network conditions

    sudo tc qdisc show dev enp7s0

### stop network impairments

Please remember do disable the network impairments after tests are done:

```
sudo tc qdisc del dev enp7s0 root netem
```

Above command will delete configuration named `root` like in above examples.