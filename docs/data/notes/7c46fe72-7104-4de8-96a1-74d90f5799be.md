
# Hashing Algorithms
## RSA
RSA security is based on 2 matching keys. There is a public key for each user, and everybody can (should) know it. There is also a private key that only the user should know. A message encrypted by the public key can only be decrypted by the private key, and visa versa.
- Thus, if I want to send you a message that only you can read, I get (from the network) your public key, encrypt the message with that key and you are the only person who can decrypt it.
- Or, if I want to prove to you that I sent a message, I can encrypt the message with my private key, tell you (in open text or in another message) how it was encrypted. Then you could decrypt the message with my public key, and if it becomes readable, you know it came from me.
- RSA-129 is a publicly available 129 digit number that is derived from multiplying 2 very large prime numbers (those numbers are unknowable)
