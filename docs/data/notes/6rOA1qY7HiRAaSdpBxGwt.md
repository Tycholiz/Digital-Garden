
SPF defines which IPs are allowed to send email for your domain.
- This allows you to instruct recipient servers on how to treat email that claims to be from your domain, but wasn’t sent by a server you approved 
    - If someone is spoofing your domain and sending mail that claims to be from you but isn’t, this is how you define the methods used to handle that.

It also improves your inbox delivery, since by including it, you are signalling that security is important.

An SPF record may or may not be required by a mail server

An SPF record is a TXT DNS record
- there is an SPF record, but it is deprecated.

The SPF record starts with `v=spf1`

The end of the record allows us to decide what you want to request of recipient servers if they receive email from your domain that doesn’t come from one of the places you specified in your SPF record.
- We have 3 options:
    - `?` - “I don’t care what you do if you get an email from a server that I haven’t approved to send from my domain. Do what you want.”
    - `~` - “If you get an email from a server I didn’t approve, it probably wasn’t me. You should do what you think is best.”
    - `-` - “If you get an email from a server that I didn’t approve, please reject it. It wasn’t from me.”
        - we should be confident in our SPF record to pick this one.
