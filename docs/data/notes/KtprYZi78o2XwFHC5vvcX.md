
CPUs have electronic clocks in them, in fact it’s a fundamental part of how they operate as the electronics within a CPU have to operate in a synchronised way.

The clock is a crystal that oscillates a predictable number of times each second when electricity is passed through it. Counting these oscillations allows the computer to measure the passing of time.
- these atomic clocks use the vibrations of atoms to keep time.

Clocks are not perfectly accurate: it *drifts* (ie. runs faster or slower than it should). As a result, one machine may be faster or slower than another machine.
- This adds an interesting wrinkle to [[distributed|deploy.distributed]] systems, and is a reason we can't depend on the clock to determine the ordering of events.
- It is possible to synchronize clocks to some degree: the most commonly used mechanism is the Network Time Protocol (NTP), which allows the computer clock to be adjusted according to the time reported by a group of servers. The servers in turn get their time from a more accurate time source, such as a GPS receiver.
- Clock drift varies depending on the temperature of the machine.

## Why use them?
The accuracy of an atomic clock matters in computing because time synchronization is critical for many computing tasks, such as 
- coordinating distributed systems, 
    - Without accurate timekeeping, distributed systems may not be able to coordinate their activities effectively, leading to issues such as data corruption, incorrect results, or system crashes.
- scheduling tasks, 
- maintaining data consistency. 

## UE Resources
- https://onezero.medium.com/the-largely-untold-story-of-how-one-guy-in-california-keeps-the-worlds-computers-on-the-right-time-a97a5493bf73