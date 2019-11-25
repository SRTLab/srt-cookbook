# File CC

The congestion control in SRT mainly relies on the value called SndPeriod, it's the required minimum time gap between sending two consecutive packets \(in \[us\]\).

## Slow Start

The congestion control in SRT mainly relies on the value called SndPeriod, it's the required minimum time gap between sending two consecutive packets \(in \[us\]\). It's not the waiting time between packets, it's rather the time expected to be between consecutive sending, that is, when the time at the moment of the packet to be sent is distant to the previous sending time by less than SndPeriod, the procedure is sleeping for the "remaining time", otherwise the packet is sent immediately. The whole congestion control relies on setting appropriate value to SndPeriod in order to achieve appropriate sending speed. The original UDT algorithm \(implemented by FileSmoother\) relies on the following rules:

## Congestion Avoidance

