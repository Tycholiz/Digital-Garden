
### MX 
Mail exchange (SMTP)
- MX Records point to the incoming smtp server for a domain
- the record indicates how email messages should be routed (in accordance with SMTP)
	- ex. When a user sends an email to john.smith@gmail.com, the *Message Transfer Agent* (MTA) sends a DNS query to identify the mail servers for that email address. The MTA establishes an SMTP connection with those mail servers, starting with the prioritized domains
- must point to an A record
- `priority` - lower number indicates preference. In the result of a send failure, the next priority domain will be attempted 
	- `mailhost1.tycholiz.com` might have `priority` of 10, and `mailhost2.tycholiz.com` might have `priority` of 20, which would mean `mailhost2` only gets used when the first message fails to send. 
	- if we use the same priority, then both servers will receive equal amount of mail (effectively a load balancer) 
