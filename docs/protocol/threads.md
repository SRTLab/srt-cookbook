# Threads

SRT reference implementation creates the following number of threads:

1. For every UDP socket (UDP multiplexer object):
    - One receiving thread
    - One sending thread
2. One TSBPD thread on every SRT socket with TSBPD mode enabled (enabled by default).
3. One garbage collector thread for the whole SRT intance (application).



