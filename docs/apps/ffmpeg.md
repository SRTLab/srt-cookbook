# FFmpeg

## Building FFmpeg with SRT

[FFmpeg supports the SRT protocol](https://www.ffmpeg.org/ffmpeg-protocols.html#srt) out of the box. To include SRT, FFmpeg project should be built with the configure flag `--enable-libsrt`. Refer to [FFmpeg's Compilation Guide](https://trac.ffmpeg.org/wiki/CompilationGuide) for the detailed build instructions for a particular platform.

Please check [Quickstart: Running SRT and FFmpeg on Ubuntu](https://www.youtube.com/watch?v=XOtUOVhussc) YouTube Video for additional instructions.

!!! note
    There is a [feedback](https://github.com/Haivision/srt/issues/1176)
    that SRT cannot be built by FFmpeg if both `--enable-libsrt` and
    `--enable-shared` options are specified.

A list of available protocols can be determined by calling `ffmpeg -protocols`.  "srt" \(or "libsrt"\) should be listed as both input and output protocol.

## General Usage

Once SRT is available, an SRT output can be specified like this:

```text
"srt://<destination_ip>:<destination_port>"
```

!!! note
    Note that FFmpeg expects SRT latency to be specified in microseconds,
    not milliseconds as it's done in the SRT library.
    The following URI sets the SRT latency to be 400 milliseconds:
    `srt://<ip>:<port>?latency=400000`.

It's recommended to use MPEG-TS as a container: `-f mpegts`.

`ffplay` should be able to play SRT streaming with `ffplay srt://<ip>:<port>`.

Piping can be done like this:

```text
srt-live-transmit srt://<ip>:<port> file://con | ffplay -f mpegts -
```

Please take into account [issue \#407](https://github.com/Haivision/srt/issues/407).

Or pipe via UDP socket on a localhost:

```text
srt-live-transmit srt://<ip>:<port> udp://127.0.0.1:<portB>
ffplay udp://127.0.0.1:<portB> -f mpegts
```

In the latest case the `-f mpegts` argument is optional.

!!! note
    `-re` [option](https://ffmpeg.org/ffmpeg.html#Advanced-options) will slow down the reading:  
    "Read input at native frame rate. Mainly used to simulate a grab device, or live input stream \(e.g. when reading from a file\). **Should not be used with ... live input streams** \(where it can cause packet loss\). By default ffmpeg attempts to read the input\(s\) as fast as possible. This option will slow down the reading of the input\(s\) to the native frame rate of the input\(s\). It is useful for real-time output \(e.g. live streaming\)".

## Examples

### FFmpeg example: Grabbing X11 desktop screen and streaming to SRT

```text
ffmpeg -f x11grab -follow_mouse centered -r 25 -s cif -i :0.0 \
       -f mpegts srt://<target_ip>:<target_port>
```

### FFmpeg example with SMPTE bars test video source

```text
ffmpeg -f lavfi -re -i smptebars=duration=60:size=1280x720:rate=30 -f lavfi -re \
       -i sine=frequency=1000:duration=60:sample_rate=44100 -pix_fmt yuv420p \
       -c:v libx264 -b:v 1000k -g 30 -keyint_min 120 -profile:v baseline \
       -preset veryfast -f mpegts "srt://127.0.0.1:4200?pkt_size=1316"
```

### FFmpeg example: RTSP to SRT

```shell
ffmpeg -i rtsp://<src_rtsp_uri> -c copy -f mpegts srt://<target_srt_uri>
```

See also [rtsp2srt](https://github.com/snowgames95/rtsp2srt) project.

### Example with FFmpeg and `srt-live-transmit` test application

Send to UDP localhost port 5000:

```text
ffmpeg -f lavfi -re -i smptebars=duration=60:size=1280x720:rate=30 -f lavfi -re \
       -i sine=frequency=1000:duration=60:sample_rate=44100 -pix_fmt yuv420p \
       -c:v libx264 -b:v 1000k -g 30 -keyint_min 120 -profile:v baseline \
       -preset veryfast -f mpegts "udp://127.0.0.1:5000?pkt_size=1316"
```

Transmit from UDP to SRT destination:

```text
./srt-live-transmit udp://:5000 srt://<ip>:<port>?mode=rendezvous
```

Receive with SRT:

```text
./srt-live-transmit srt://<ip>:<port>?mode=rendezvous file://con > ./received.ts
```

Receive SRT and pass to `/dev/nul`:

```text
`./ffmpeg -i srt://<ip>:<port> -f null /dev/null`
```
