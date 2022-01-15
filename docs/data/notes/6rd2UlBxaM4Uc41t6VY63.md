
Load testing is the practice of modeling the expected usage of a software program by simulating multiple users accessing the program concurrently.

For example we can load test a multi-user system on a client-server architecture. But we can also load test programs like Microsoft Word or Sketch by forcing them to read very large documents. We could test a personal finance software by forcing it to generate a report based on several years' worth of data

The most accurate load testing simulates actual use, as opposed to testing using theoretical or analytical modeling.

Load testing tools analyze the entire [[OSI|network.osi-model]] protocol stack
- contrast with most regression testing tools, which focus on GUI performance
- ex. a regression testing tool will record and playback a mouse click on a button on a web browser, but a load testing tool will send out hypertext the web browser sends after the user clicks the button.
	- In a multiple-user environment, load testing tools can send out hypertext for multiple users with each user having a unique login ID, password, etc.

load testing tools provide insight into the causes for slow performance, whether it be caused by...
- Application server(s) or software
- Database server(s)
- Network – latency, congestion, etc.
- Client-side processing
- Load balancing between multiple servers

When the load placed on the system is raised beyond normal usage patterns to test the system's response at unusually high or peak loads, it is known as stress testing.
- in which case the load is usually so great that error conditions are the expected result, but there is no clear boundary when an activity ceases to be a load test and becomes a stress test.

The term "load testing" is often used synonymously with concurrency testing, software performance testing, reliability testing, and volume testing for specific scenarios.
- All of these are types of non-functional testing that are not part of functionality testing used to validate suitability for use of any given software.

### What a Load Testing framework/tool does
The following procedure happens across all load testing tools and frameworks:

1. When customers visit your website, a script recorder records the communication and then creates related interaction scripts.
2. A load generator tries to replay the recorded scripts, which could possibly be modified with different test parameters before replay.
3. In the replay procedure, both the hardware and software statistics will be monitored and collected by the conductor.
	- these statistics include the CPU, memory, disk IO of the physical servers and the response time, the throughput of the system under test (SUT), etc.
4. At last, all these statistics will be analyzed and a load testing report will be generated.

### Shopping cart example
A website with shopping cart capability is required to support 100 concurrent users broken out into the following activities:

- 25 virtual users log in, browse through items and then log off
- 25 virtual users log in, add items to their shopping cart, check out and then log off
- 25 virtual users log in, return items previously purchased and then log off
- 25 virtual users just log in without any subsequent activity

A test analyst can use various load testing tools to create these VUsers and their activities. Once the test has started and reached a steady-state, the application is being tested at the 100 VUser loads as described above. The application's performance can then be monitored and captured.

### Analysis
The nth percentile is important because it is usually how you set up your "failure" threshold. If you decide that you want your service to respond within 500ms "most of the time" then you would set your toleration for the 90% threshold to be 500ms. This lets you push your load as far as it can go within this toleration limit.
- ex. you have a service `WidgetService`. You set up your toleration to be 500ms. Then you start to bombard `WidgetService`. At 80 requests per second, `WidgetService` starts to have 88% of its requests take less than 500ms, but 12% take more than that. This would be considered a fail.
	- By analyzing what your % spread looks like at various numbers of requests per second, you can determine how many instances of `WidgetService` you need. This is why the nth percentile is important.
	- If it turns out that you have to guarantee < 200ms requests and 10% of your requests in an interval fail this at 20 requests per second, then that means 2 times per second you are just "not good enough." If you optimize your balancing / routing and never have a sub-par request, then you've done your job... but you can't adequately do your job if you have no data.

### Tools/Frameworks
K6
