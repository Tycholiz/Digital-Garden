
# ELB (Elastic Load Balancing)
Comes in 3 variants:
- Classic
	- Legacy software.
- Application (ALB)
	- a proper reverse proxy that sits between the internet and your application
	- Every request to your application gets handled by the load balancer first. The load balancer then makes another request to your application and finally forwards the response from your application to the caller.
- Network (NLB)
	- behave like load balancers, but they work by routing network packets rather than by proxying HTTP requests
