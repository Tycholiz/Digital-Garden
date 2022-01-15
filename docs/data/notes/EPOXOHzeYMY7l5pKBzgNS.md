
Timestamps are often recorded in GMT.
- GMT is 8 hours ahead of PST.

# Overview
we need to express both a time and a location whenever we want to place some event
- The basic problem is that we assume spatial coordinates more than we assume temporal coordinates
	- Consider how timezones really are. Everyone has a concept of 3:00 in the afternoon. It doesn't matter which timezone you are in, because 3:00 will have a certain feel. We ignore the timezone (spatial coordinate) when it comes to conceptualizing the time. It falls secondary to the time (temporal coordinate).
- This problem results in people having a poor conception of how their timezone relates to others. This is why `-07:00` is a pretty abstract concept, and thus meaningless to most people.
- When talking about time, people almost always leave out some crucial information, leaning on the context of what they are saying to help communicate.
	- ex. to denote a year people often shorten to 2 digits ('04).

### Other issues
- Scientific time naturally has origin 0, as usual with scientific measures, even though the rest of human time notations tend to have origin 1

#### Unix time
The Unix-time way of solving time-related problems is to set time 0 equal to 1970-01-01 00:00:00 UTC. This approach works well as long as the rules for converting between relative and absolute time are stable. As it turns out, they are not.
- Some have used local time as the point of reference, some use decoded local time as the reference, and some use hardware clocks that try to maintain time suitable for direct human consumption. Without consistency, problems arise.

#### Political Time
On the surface, answering the question "how long does it take for a clock to show the same value?" is an easy one to answer. The answer is 24 hours, right? Well, this question gets complex when we consider daylight savings, since 1/365 days will have 23 hours, and 1/365 days will have 25 hours.

There is no way you can know from your location alone which time zone applies at some particular point on the face of the earth: you have to ask the people who live there what they have decided.

Unfortunately, political time is what people want their computers to produce when asking for time of day.

# UE Resources
[Essential reading before working with Timezones](https://zachholman.com/talk/utc-is-enough-for-everyone-right)
- Jensen recommends using https://date-fns.org/

# E Resources
[Scientific paper on time and its complexities](http://naggum.no/lugm-time.html)
