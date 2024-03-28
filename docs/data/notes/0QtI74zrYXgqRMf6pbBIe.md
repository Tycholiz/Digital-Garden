
Mail exchange (SMTP)
- MX Records point to the incoming smtp server for a domain
- the record indicates how email messages should be routed (in accordance with SMTP)
	- ex. When a user sends an email to john.smith@gmail.com, the *Message Transfer Agent* (MTA) sends a DNS query to identify the mail servers for that email address. The MTA establishes an SMTP connection with those mail servers, starting with the prioritized domains
- must point to an A record
- `priority` - lower number indicates preference. In the result of a send failure, the next priority domain will be attempted 
	- `mailhost1.tycholiz.com` might have `priority` of 10, and `mailhost2.tycholiz.com` might have `priority` of 20, which would mean `mailhost2` only gets used when the first message fails to send. 
	- if we use the same priority, then both servers will receive equal amount of mail (effectively a load balancer) 
	- priority exists to prevent mail-routing loops

MX records specify a mail exchanger for a domain name: a host that will either process or forward mail for the domain name (through a firewall, for example)
- Processing the mail means either delivering it to the individual to whom it’s addressed or gatewaying it to another mail transport, such as X.400
- Forwarding means sending it to its final destination or to another mail exchanger closer to the destination via [[SMTP|protocol.SMTP]]