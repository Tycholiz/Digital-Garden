
A Buffer is an in-memory (typically RAM) collection of raw bytes
- Because computers store data in bytes, a simple way to think of a Buffer is to think of it as an array of bytes:
```js
const buffer = [11111111, 11011001, 10010001]
```

The point of a Buffer is to simply store anything as an array of bytes. The reason this is important is because every thing in computing communicates in bytes.

- It can be thought of as a temporary place to put things that need to be worked on or processed,
    - ex. like a to-do pile of work on your desk: Instead of just doing work the moment it’s given to you, you have people put it in a folder on your desk so you can work on everything at a steady pace
- Buffering is a *technique*, not one specific place in the computer.

### with Streams
a Buffer is what you get from or put to a Stream. The stream then reads or writes the buffer to the input or output target that the Stream is connected to.

### Examples

#### Movies
The rate at which you download a file may fluctuate but if the playback of that video did too it would be very awkward to watch. A buffer is a temporary storage place for some of that video so the downloading process can put it somewhere as it comes in and the playback process can play it at a steady rate.
- For video, what you do is to first retrieve (for example 10 seconds) and then start to play. If the network drops some packet (and the data along with it) you can ask for it to be retransmitted to fix the problem.
- if the buffer empties without having an end-of-file marker (a special flag designed to tell the playback device that no more data is needed), the program won’t know what data to display/playback next. Thus in the context of video/audio, buffering is when the video/audio buffer is filled with data when it has processed all the data in the buffer without receiving an end-of-file marker.
- When you see a “buffering” message in the middle of playing a video, that means that the internet got so slow that the player ran out of video to display. A smart player might notice that 10 seconds (or whatever) of buffered video wasn’t enough, given how flaky the internet is at this location is, so maybe it’ll bump up the buffer to 20 or 30 seconds.

#### Keyboard
When you type on your keyboard, the keystroke data is put into a buffer. Then depending on what program, text box, etc. you are using, the processor processes the data and makes the appropriate updates to where it is needed (like showing it on screen, applying any commands or special processes to it like copy and paste functions, storing the information in RAM/ROM, etc.) Typically each process has a separate buffer which your CPU can access, handle reading from, writing to, and processing, as well as share information between other buffers. i.e. your keyboard has one buffer, your mouse another, your display another, your web browser another etc. and your processor handles information sharing between all of them.
