
A Scheduler lets you define in what execution context will an Observable deliver notifications to its Observer.

A scheduler controls when a subscription starts and when notifications are delivered. It consists of three components.
1. A Scheduler is a data structure. It knows how to store and queue tasks based on priority or other criteria.
2. A Scheduler is an execution context. It denotes where and when the task is executed (e.g. immediately, or in another callback mechanism such as setTimeout or process.nextTick, or the animation frame).
3. A Scheduler has a (virtual) clock. It provides a notion of "time" by a getter method now() on the scheduler. Tasks being scheduled on a particular scheduler will adhere only to the time denoted by that clock.

[docs](https://rxjs.dev/guide/scheduler)