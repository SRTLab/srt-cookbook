# Using NetEm to Emulate Networks

[NetEm](https://www.linux.org/docs/man8/tc-netem.html) (Network  Emulator) is an enhancement of the Linux traffic control facilities that allow adding delay, packet loss, duplication and other characteristics to packets outgoing from a selected network interface.  NetEm uses the existing Quality Of Service (QOS) and Differentiated Services (diffserv) facilities in the Linux kernel.

It's a great tool to test SRT connections with network impairments like on real networks.

## Installation 

Linux traffic control (tc) and NetEm (netem) are available in most Linux distributions, typically as part of the `iproute2` package.

On **Ubuntu** systems the `iproute2` package (including netem) can be installed using following command:

```
sudo apt-get install iproute2
```

### Installation on CentOS 8.2

On **CentOS 8.2** the `iproute-tc` package (including netem) can be installed using following command:

```
sudo yum install iproute-tc
```

**NOTE**: RHEL-based systems like CentOS 8.2 might need an additional kernel module in order to run netem. This would be indicated if, in running netem, the look-up of the name for `qdisc` fails, like in example shown below:

```
sudo tc qdisc add dev enp7s0 root netem delay 10ms 10ms loss 2% rate 12mbit
Error: Specified qdisc not found.
```

In such a case, install the modules `kernel-modules-extra` and `kernel-debug-modules-extra` with following command:

```
sudo yum install kernel-debug-modules-extra kernel-modules-extra
```

After installation a reboot is needed to activate the kernel modules.

## Getting Started with TC & NetEm

The following examples use the interface `enp7s0`. Modify to match your network adapter (e.g. `eth0`).

To be able to change settings, root privileges are required.

### Adding Delay

**NOTE**: netem only adds delay to packets leaving the interface. If you want to simulate bi-directional delay, two instances of tc netem - one on each end - are required. 

The following command adds 250 ms of delay to packets leaving interface `enp7s0`:

```
sudo tc qdisc add dev enp7s0 root netem delay 250ms
```

### Adding Delay with Volatility

The following command sets the transmission of enp7s0 network interface to be delayed by 100ms + 10ms (any value between 90 and 110 ms):

```
sudp tc qdisc add dev enp7s0 root netem delay 100ms 10ms
```

The volatility (randomness) of such fluctuations can be further  specified. The following command sets network interface enp7s0  transmission to 100 ms, while 30% of the packets selected at random are delayed by +10 ms:

```
sudo tc qdisc add dev enp7s0 root netem delay 100ms 10ms 30%
```

### Adding Packet Loss

The following command sets the enp7s0 network interface transmission to randomly drop 1% of the packets:

```
sudo tc qdisc add dev enp7s0 root netem loss 1%
```

### Adding Packet Loss with Success Rate

```
sudo tc qdisc add dev enp7s0 root netem loss 1% 30%
```

### Duplicated Packets

```
sudo tc qdisc add dev enp7s0 root netem duplicate 1%
```

### Corrupted Packets

For Linux kernel versions above 2.6.16, netem can also simulate damage to packets. The following command randomly damages 0.2% of the outgoing packets:

```
sudo tc qdisc add dev enp7s0 root netem corrupt 0.2%
```

### Packet Reordering

The following command sets network interface enp7s0 to send out 25% of data packets (with 50% relevance) immediately and delay all other packets by 10 ms.

```
sudo tc qdisc add dev enp7s0 root netem delay 10ms reorder 25% 50%
```

### View the Configured Network Conditions

    sudo tc qdisc show dev enp7s0

### Stopping Network Impairments

**IMPORTANT**: Please remember to disable the network impairments after tests are done by using the following command:

```
sudo tc qdisc del dev enp7s0 root netem
```

The above command will delete the `root` configuration that was used in the examples above.

## NetEm Wrapper

We provide a netem wrapper written in python to concatenate network conditions by means of simple configuration files. This wrapper is available in [SRTLab/netem-wrapper](https://github.com/SRTLab/netem-wrapper).

Here is an example of a config file that can be created with it:

```
{
  "name": "example",
  "interface": "wlp3s0",
  "events": {
    "1": {
      "duration": 5000,
      "rules": [
        "clear"
      ]
    },
    "2": {
      "duration": 15000,
      "rules": [
        "delay 30ms 10ms distribution normal",
        "loss 0.1% 0.25%"
      ]
    },
    "3": {
      "duration": 10000,
      "rules": [
        "delay 50ms"
      ]
    },
    "4": {
      "duration": 5000,
      "rules": [
        "clear"
      ]
    }
  }
}
```

The wrapper has an optional parameter that outputs a json file with the timestamps of when the conditions were applied, in case it wants to be used for later analysis.

## Further Reading

These were just some basic examples to get started. TC & NetEm allow complex simulations and more information can be found in the documents listed below:

- [Emulating WAN with NETEM II: Packet Loss, Duplication, Reordering, and Corruption](http://ce.sc.edu/cyberinfra/workshops/Material/NTP/Lab%204.pdf) (PDF by Universit of South Carolina)

- [Traffic Control Manual](https://www.cs.unm.edu/~crandall/netsfall13/TCtutorial.pdf) (PDF by University of New Mexico)

