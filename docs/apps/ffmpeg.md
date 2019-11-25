# FFmpeg

FFmpeg supports SRT protocol out of the box. But it needs to be built with `-enable-protocol=libsrt` to include SRT.

Refer to [FFmpeg's Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide) for build instructions for your platform.

A list of the available protocols can be determined by calling `ffmpeg -protocols`.  "srt" \(or "libsrt"\) should be listed both as an input, and as an output protocol.

Once SRT is available, an SRT output can be specified like this:

```text
"srt://<destination_ip>:<destination_port>"
```

It would be a good idea to use MPEG-TS as a container `-f mpegts`.

`ffplay` should be able to play SRT streaming with `ffplay srt://<ip>:<port>`.

Piping can be used as well like this:

```text
srt-live-transmit srt://<ip>:<port> file://con | ffplay -f mpegts -
```

Please note the [issue \#407](https://github.com/Haivision/srt/issues/407).

Or pipe via UDP socket on a localhost:

```text
srt-live-transmit srt://<ip>:<port> udp://127.0.0.1:<portB>
ffplay udp://127.0.0.1:<portB> -f mpegts
```

In the latest case the `-f mpegts` argument is optional.

**Note!**  
`-re` [option](https://ffmpeg.org/ffmpeg.html#Advanced-options) will slow down the reading:

> Read input at native frame rate. Mainly used to simulate a grab device, or live input stream \(e.g. when reading from a file\). **Should not be used with ... live input streams** \(where it can cause packet loss\). By default ffmpeg attempts to read the input\(s\) as fast as possible. This option will slow down the reading of the input\(s\) to the native frame rate of the input\(s\). It is useful for real-time output \(e.g. live streaming\).

## FFmpeg example grabbing X11 desktop screen and streaming to SRT

```text
ffmpeg -f x11grab -follow_mouse centered -r 25 -s cif -i :0.0 \
       -f mpegts srt://<target_ip>:<target_port>
```

## FFmpeg example with SMPTE bars test video source

```text
ffmpeg -f lavfi -re -i smptebars=duration=60:size=1280x720:rate=30 -f lavfi -re \
       -i sine=frequency=1000:duration=60:sample_rate=44100 -pix_fmt yuv420p \
       -c:v libx264 -b:v 1000k -g 30 -keyint_min 120 -profile:v baseline \
       -preset veryfast -f mpegts "srt://127.0.0.1:4200?pkt_size=1316"
```

## Example with FFmpeg and srt-live-transmit

Send to UDP localhost port 5000

```text
ffmpeg -f lavfi -re -i smptebars=duration=60:size=1280x720:rate=30 -f lavfi -re \
       -i sine=frequency=1000:duration=60:sample_rate=44100 -pix_fmt yuv420p \
       -c:v libx264 -b:v 1000k -g 30 -keyint_min 120 -profile:v baseline \
       -preset veryfast -f mpegts "udp://127.0.0.1:5000?pkt_size=1316"
```

Transmit from UDP to SRT destination

```text
./srt-live-transmit udp://:5000 srt://<ip>:<port>?mode=rendezvous
```

Receive with SRT

```text
./srt-live-transmit srt://<ip>:<port>?mode=rendezvous file://con > ./received.ts
```

Receive srt and pass to dev/nul

```text
`./ffmpeg -i srt://<ip>:<port> -f null /dev/null`
```

