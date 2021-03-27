# TSDuck

TSDuck is an extensible toolkit for MPEG/DVB transport streams. TSDuck is used in digital television systems for test, monitoring, integration, debug, lab, demo.

In practice, TSDuck is used for:

* Transport stream acquisition or transmodulation, including DVB, ATSC, ISDB, ASI and IP multicast.
* Analyze transport streams, PSI/SI signalization, bitrates, timestamps.
* Monitor and report conditions on the stream (video and audio properties, bitrates, crypto-periods, signalization).
* On-the-fly transformation or injection of content and signalization.
* Modify, remove, rename, extract services.
* Work on live transport streams, DVB-S/C/T, ATSC, ASI, IP-TV, HTTP, HLS, SRT or offline transport stream files.
* Re-route transport streams to other applications.
* Receive from or send to specialized hardware such as cheap tuners, Dektec or HiDes devices and modulators.
* Extract or inject MPE, SCTE 35, extract Teletext, T2-MI.
* And more...

For detailed information, guidelines and use cases, please visit [TSDuck website](https://tsduck.io/).

## TSDuck Installation on Ubuntu 20.04 LTS
<!-- Restructure and put in the very end -->

This is a little how-to on building TSduck for Ubuntu 20.04 LTS with SRT support.

### Build from Source

#### Building instructions 

See building instructions at: [https://tsduck.io/doxy/building.html](https://tsduck.io/doxy/building.html)

```
mkdir tsduck
cd tsduck
git clone https://github.com/tsduck/tsduck.git
```

Execute the shell-script:

```
build/install-prerequisites.sh
```

It downloads and installs the requested packages which are necessary to build TSDuck. The list of packages and how to install them depend on the operating system distribution and version.

#### Building without specialized dependencies

In specific configurations, you may want to disable some external libraries such as `libcurl` or `pcsc-lite`. Of course, the corresponding features in TSDuck will be disabled but the impact is limited. For instance, disabling `libcurl` will disable the plugin `http` (the plugin will still be there but it will report an error when used).

The following `make` variables can be defined:

- `NOTEST` : Do not build unitary tests.
- `NODTAPI` : No Dektec support, remove dependency to `DTAPI`.
- `NOCURL` : No HTTP support, remove dependency to `libcurl`.
- `NOPCSC` : No smartcard support, remove dependency to `pcsc-lite`.
- `NOSRT` : No SRT (Secure Reliable Transport), remove dependency to `libsrt`.
- `NOTELETEXT` : No Teletext support, remove teletext handling code.

```
make NOPCSC=1 NOCURL=1 NODTAPI=1
```

To speed up the compilation time a optional number of parallel threads can be set with the option `-j10` make would use 10 parallel processes:

```
make -j10 NOPCSC=1 NOCURL=1 NODTAPI=1
```

#### Set PATH to run from directory

On all Unix systems, the binaries, plugins and tests are built inside the `bin` subdirectory.

To run a tool from its build directory, a few environment variables shall be defined (including `$PATH`). A shell-script named `setenv.sh` in the `build` directory defines the appropriate environment for running binaries.

Execute:

```
source build/setenv.sh
```

Note the usage of the `source` command to make sure that the environment variables are defined in the current shell.

Example: 

```
$ source build/setenv.sh 
$ which tsp
~/tsduck/bin/release-x86_64-vmubuntu/tsp
$ tsp --version
tsp: TSDuck - The MPEG Transport Stream Toolkit - version 3.26-2313
```

#### Optionally install TSDuck on system

```
sudo make install
```

#### Cleaning up

On Linux and macOS, the same cleanup task is achieved using the following command: 

```
make distclean
```

## How to replay a .ts file as a stream
<!-- Change the title -->
<!-- Some intro -->

### 1. Use `tsanalyze` to find out a bitrate of the stream

```
tsanalyze /home/n00b/Videos/StressTest_4KP25_h264.ts 

===============================================================================
|  TRANSPORT STREAM ANALYSIS REPORT                                           |
|=============================================================================|
|  Transport Stream Id: .......... 1 (0x0001)  |  Services: .............. 1  |
|  Bytes: ....................... 349,332,952  |  PID's: Total: .......... 4  |
|  TS packets: .................... 1,858,154  |         Clear: .......... 4  |
|     With invalid sync: .................. 0  |         Scrambled: ...... 0  |
|     With transport error: ............... 0  |         With PCR's: ..... 1  |
|     Suspect and ignored: ................ 0  |         Unreferenced: ... 0  |
|-----------------------------------------------------------------------------|
|  Transport stream bitrate, based on ....... 188 bytes/pkt    204 bytes/pkt  |
|  User-specified: ................................... None             None  |
|  Estimated based on PCR's: ............... 55,933,494 b/s   60,693,792 b/s  |
                                             ^^^^^^^^^^^^^^
                                             ||||||||||||||
                                             bitrate of the stream
```

### 2. Replay `.ts` file as MPEG-TS UDP stream

```
tsp -I file --infinite /home/n00b/Videos/StressTest_4KP25_h264.ts -P regulate --bitrate 55933496 -O ip 192.168.2.49:4904
```

The specification of bitrate of the stream can also be set automatically without use of the `--bitrate` option:

```
tsp -I file --infinite /home/n00b/Videos/StressTest_4KP25_h264.ts -P regulate -O ip 192.168.2.49:4904
```

### 3. SRT as input or output
<!-- Something is missing in the title -->

Up to TSDuck version 3.25, the `srt` input plugin used the caller mode and the `srt` output plugin used the listener mode.

From TSDuck version 3.26 onwards, the `srt` input and output plugins can indifferently use caller, listener or rendezvous modes, using the same consistent set of command line options. See the [TSDuck user's guide](https://tsduck.io/download/docs/tsduck.pdf) for a complete reference.

The following examples are based on version 3.26.

### 4. Replay `.ts` file as SRT-Listener stream
<!-- Title -->

Now it's time to get started. There are some differences with the TSDuck SRT implementation compared to other SRT applications. Typically you only specify a port without IP like `srt://:4900` and a SRT-listener on port 4900 will be created. If you specify an IP:port combination, SRT typically would act as SRT-caller to that IP:port (e.g. `srt://192.168.2.49:4900`)

With TSDuck, you can create a listener port and optionally specify the adapter, which it should listen on using `--listener port` or `--listener interface:port`. In this test the Linux machine has two interfaces and the one with IP `192.168.2.3` was chosen to act as listening interface.

```
tsp -I file --infinite /home/n00b/Videos/StressTest_4KP25_h264.ts -P regulate -O srt --listener 192.168.2.3:4900 --transtype live --messageapi
```

It is also possible to just specify the port without a particular interface to use:

```
tsp -I file --infinite /home/n00b/Videos/StressTest_4KP25_h264.ts -P regulate -O srt --listener 4900 --transtype live --messageapi
```

A detail worth mentioning: `-O srt --listener "[IP:]Port"` for `tsp` already specifies the usage of the SRT protocol. So there is no need to add the `srt://` prefix to the IP or DNS-name. Adding it would actually result in an error.

Now another SRT application can connect to that SRT stream on `192.168.2.3:4900` provided by `tsp`, e.g., [srt-live-transmit](https://github.com/Haivision/srt/blob/master/docs/srt-live-transmit.md) sample application to flip it back to MPEG-TS.

```
srt-live-transmit.exe srt://192.168.2.3:4900 udp://192.168.2.49:10002
```

Or VLC player: press `CTRL+N` to open a new network stream and type in URL `srt://192.168.2.3:4900` to connect to the stream in this example.

## Analyzing incoming SRT stream

Incoming SRT stream containing MPEG-TS one can also be analyzed. The following example would connect in caller mode to the SRT stream, which was sent out using `tsp` as described in the section above and perform an analysis for 10 seconds:

```
tsp -I srt --caller 192.168.2.3:4900 --transtype live --messageapi -P until --seconds 10 -P analyze -O drop
```

It would generate an output to `stdout` like this (shortened)

```
===============================================================================
|  TRANSPORT STREAM ANALYSIS REPORT                                           |
|=============================================================================|
|  Transport Stream Id: .......... 1 (0x0001)  |  Services: .............. 1  |
|  Bytes: ........................ 99,391,840  |  PID's: Total: .......... 4  |
|  TS packets: ...................... 528,680  |         Clear: .......... 4  |
|     With invalid sync: .................. 0  |         Scrambled: ...... 0  |
|     With transport error: ............... 0  |         With PCR's: ..... 1  |
|     Suspect and ignored: ................ 0  |         Unreferenced: ... 0  |
|-----------------------------------------------------------------------------|
|  Transport stream bitrate, based on ....... 188 bytes/pkt    204 bytes/pkt  |
|  User-specified: ......................... 69,642,777 b/s   75,569,821 b/s  |
|  Estimated based on PCR's: ............... 58,771,501 b/s   63,773,331 b/s  |
|-----------------------------------------------------------------------------|
|  Broadcast time: .................................... 11 sec (0 mn 11 sec)  |
|-----------------------------------------------------------------------------|
|  Srv Id  Service Name                              Access          Bitrate  |
|  0x0001  Service01 .................................... C   67,660,510 b/s  |
|                                                                             |
|  Note 1: C=Clear, S=Scrambled                                               |
|  Note 2: Unless specified otherwise, bitrates are based on 188 bytes/pkt    |
===============================================================================
...
```
Output was cut off here. For more details perform this on your stream or see
the [TSDuck user's guide](https://tsduck.io/download/docs/tsduck.pdf).
