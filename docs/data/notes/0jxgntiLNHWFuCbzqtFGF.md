
Never send to generic emails, like `info@[businessname].com`. This hurts your sender reputation score

The inbox of a recipient needs to authenticate the source of the email (usually with SPF or DKIM)

SPF & DKIM are authentication systems that tell Internet Service Providers (ISPs), like Gmail and Yahoo, that incoming mail has been sent from an authorized system, and that it is not spam or email spoofing.

SPF & DKIM authentication cannot be done for free webmail accounts like Google, Yahoo, and Hotmail.

There is only one SPF record per domain. (If you have more than one SPF DNS record, ISPs will not know which one to use which may cause authentication issues.)
- multiple DKIM records is fine

With SMTP you are sending, relaying, or forwarding messages from a mail client (like Microsoft Outlook) to a receiving email server. A sender will use an SMTP server to carry out the process of transmitting an email message.

### IMAP (Internet Access Message Protocol) 
IMAP is an email protocol that deals with managing and retrieving email messages from the receiving server.
1. After creating an email and pressing ‘send’, your email client (e.g. Gmail, Thunderbird, Outlook, etc.) will use SMTP to send your message from your email client to an email server.
2. Next, the email server will use SMTP to transmit the message to the recipient’s receiving email server.
3. Upon a successful receipt of the SMTP transmission (indicated by a 250 OK response code), the recipient’s email client will fetch the message using IMAP and place it in the inbox for the recipient to access.

### Pop3
POP3 downloads the email from a server to a single computer, then deletes the email from the server.

On the other hand, IMAP stores the message on a server and synchronizes the message across multiple devices.

# UE Resources
[Email Sender Reputation](https://www.sparkpost.com/resources/email-explained/email-sender-reputation/)
