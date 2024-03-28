
Transcoding, encoding, and decoding are various terms used when a media file needs to be transformed to be better suited for a specific device, internet connection, or hardware

The two main factors in streaming is the CPU (GPU) and internet bandwidth

### Transcoding
Imagine we had a movie on our NAS in the HEVC format, and we wanted to play it back on our iPhone which doesn't support that format.
- in this case, the Plex Media Server (on the NAS) will try to change the file to a more suitable version on the fly. This is transcoding.
    - this work is done by the NAS CPU, and takes a heavy toll on the CPU
- In streaming apps like Plex, we can enable an option to allow the CPU to run to its full potential (`Transcoder Quality`: `Make My CPU Hurt` option in Plex)

Locally, transcoding probably isn't necessary.
- when accessing media from remote locations with limited bandwidth, weaker internet connection, or smaller devices, you may want to access the media in lower quality, which is where transcoding (or real-time encoding), comes in.

CPU needs for transcoding:
- In Intel or AMD Based Based CPU that is 64bit (x86) in Architecture
- Higher than 1.6Ghz in Frequency
- More than 2 Cores

* * *

### Bitrate
Bitrate refers to the quantity of data that is processed per unit of time
- ex. megabits per second (Mbps) for video 
- ex. kilobits per second (kbps) for audio.

The higher the bitrate, the better the quality and resolution of the media tends to be.