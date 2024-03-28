
DKIM (Domain Keys Identified Mail) is an email authentication technique that allows the receiver to check that an email was indeed sent and authorized by the owner of that domain. 
- Therefore DKIM is supposed to reduce spoofing.
- This is done by giving the email a digital signature. This DKIM signature is a header that is added to the message and is secured with encryption.

Once the email receiver (or receiving system) determines that an email is signed with a valid DKIM signature, it is confident that nothing in the email (body, subject, attachments etc) has been modified.

DKIM signatures are validated on the server, and thus aren't visible to end users.
