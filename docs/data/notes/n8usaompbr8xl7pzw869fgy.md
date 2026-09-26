
A window is a way to chunk data so that it can be processed in a more effective way.

## Window processing patterns
### Tumbling Windows
Elements are assigned to a window based on their timestamp.

- each window has a specified size 
- windows do not overlap with each other.

![](/assets/images/2023-01-10-13-10-03.png)

### Sliding Windows
- see: [[algorithm implementation|general.algorithms.patterns.sliding-window]]

Like tumbling windows, each window has a specified size.

Sliding windows allow you to configure how frequently a window is started.
- therefore, sliding windows can be overlapping if the slide is smaller than the window size, causing elements to be assigned to multiple windows.
- ex. you could have windows of size 10 minutes that slides by 5 minutes. With this you get every 5 minutes a window that contains the events that arrived during the last 10 minutes as depicted by the following figure.

![](/assets/images/2023-01-10-13-21-31.png)

### Session Window
assigner groups elements by sessions of activity.

- do not overlap
- do not have a fixed start and end time
- Instead, a session window closes when it does not receive elements for a specified period of time (ie. when a gap of inactivity occurred).
  - When this period expires, the current session closes and subsequent elements are assigned to a new session window.

![](/assets/images/2023-01-10-13-28-01.png)

### Global Window
With a global window scheme, all elements with the same key will be assigned to the same single global window

![](/assets/images/2023-01-10-13-29-40.png)