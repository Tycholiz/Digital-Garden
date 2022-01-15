
## The Process

The MTA receives the email from the MSA, which it receives from the MUA
- Once the MTA gets the email, relaying comes into play— with the MTAs acting as the relays (SMTP relay)
	- In relaying, the email is forwarded to other MTAs if the recipient of the email is not hosted locally on the same system as the sender.
- Once the relaying process has finished, the email is sent to the Mail Delivery Agent (MDA)— which is the last stop before being delivered to a client's mailbox
- The whole process uses SMTP, except for the final stage, which is between MDA and and MUA, which uses POP3 or IMAP
	- In other words, the process of getting mail from the "mail server" onto the email client uses POP3/IMAP.

![](/assets/images/2021-04-07-10-12-35.png)
- MUA - Mail User Agent (email client)
- MSA - Mail Submission Agent
- MDA - Mail Delivery Agent
- MTA - Mail Transfer Agent

### Mail Transport Agent (MTA)
A mail server can have many names: mail relay, mail router, Internet mailer. But the most common alias is an mail transport agent (MTA)
- an MTA is responsible for transferring email among users on the internet
- MTAs query the MX records and select a mail server to transfer emails
- Usually, MTAs use a store-and-forward model of mail handling. This means that outgoing mail is put into a queue and waits for the recipient’s server response. An MTA will recurrently try to send emails. If the mail fails to be delivered during the established term, it will be returned to the mail client.

#### MTA impact on email deliverability
There are three major factors that email deliverability is based on:
- sender’s reputation
- infrastructure & authentication
- content

The reputation of the domain and IP address the email is sent from is the most important thing.
- MTAs can protect and strengthen the reputation of the sender.

##### Brand new IPs
If you’re building your reputation from scratch, you should not use your virgin IP address at full load. It has no email sending history and thus needs some warming up. An MTA will let you do this and later slowly increase sending capacity

* * *

## Breakdown of SMTP
1. Alice sends an email to Bob. 
2. In reality, it goes directly to her mail server, where it is placed in a message queue.
3. The client-side of SMTP (running on Alice's mail server) sees the message sitting in the queue.
4. In response, it opes up a TCP connection to an SMTP server running on Bob's machine
5. After the handshaking process is complete, the SMTP client sends Alice's message into the TCP connection.
6. The message is received by Bob's mail server. The server then places the message in Bob's mailbox.
