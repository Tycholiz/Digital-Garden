
A public key certificate is a file whose purpose is to verify the validity of a public key.
- a certificate *contains* a public key.

Think about the public key as a lock-only mechanism, so you need to know the private key to unwrap, unlock or decode data again.

The certificate contains information about:
- the public key
- the identity of the public key's owner (called the *subject*)
- the digital signature of an authority that has verified the certificate's contents (the *issuer*)

Typically, a certificate is itself signed by a certificate authority (CA) using CA's private key. This verifies the authenticity of the certificate.

When the issuer can confirm that the digial signature is valid (ie. when the issuer *signs* the message), it will have effectively authenticated the other party (the subject). That is, it is highly confident that the other party is who they say they are.
- also, the issuer knows that the message was never altered in transit (*integrity*).

spec: the certificate is used with the public key to sign the message. Therefore, the issuer can be assured that the message that was sent to them originates from the correct identity.

### Types of certificate
- [[https.TLS.certificates]]
- Email certificate
- Self-signed certificate - a certificate with a `Subject` that matches its issuer, and a signature that can be verified by its own public key.
	- a self-signed certificate is mostly useless, but a the digital certificate chain of trust must start with a self-signed certificate (ie. a *root certificate*). A [[certificate authority|https.TLS.certificates.certificate-authority]] self-signs a root certificate to be able to sign other certificates.
- Intermediate certificate - has a similar purpose to the root certificate; its only use is to sign other certificate, but the intermediate certificate is not self-signed; it must be signed by either a root certificate or another intermediate certificate.
- Code signing certificate - sign binaries so the identity of the developer is verified as the one who created the binary.

### Public Key Infrastructure (PKI)
The PKI is the underlying system needed to create, manage, distribute, use, store and revoke digital certificates and manage public-key encryption.
- at its core, it is a set of roles, policies, hardware, software and procedures that enable us to create/manage/distribute the keys.

The PKI performs trust services, meaning it serves to trust the actions/outputs of entities, whether they are computers or people.
- the objective of a trust service is 1+ of the following:
	- Confidentiality - Assurance that no entity can maliciously or unwittingly view a payload in clear text
	- Integrity - Assurance that it would be obvious if an entity tampered with transmitted data
	- Authenticity - Assurance that you have certainty of what you are connecting to (server-side authentication; typically used when authenticating to a web server using a password), or evidencing your legitimacy when connecting to a protected service (client-side authentication; sometimes used when authenticating using a smart card, ie. hosting a digital certificate and private key).

### Difference between Symmetric encryption and Asymmetric encryption
In a simple cryptographic system, I have some message that I can encrypt using a key. I can then give you that key along with the information, and you can decrypt it and read the message.
- This is known as Symmetric Encryption
- The problem with this, is that I need to send you the key. Since we have not yet established a secure connection, we cannot be sure that no one is listening in.

In Public Key Cryptography, we generate a key pair: a private key and public key.
- This is known as Asymmetric Encryption
- It is Asymmetric because one key is used to encrypt the message, while the other is used to decrypt it.
	- ie. Anything we encrypt with key A can only be decrypted with key B, and anything encrypted with key B can only be decrypted with key A
- Now we can have a situation where both you and I have our own key-pairs, with each person's public key being widely distributable. 
	- Now if I want to send something to you, I just need your public key. I can encrypt the message like this, and you are the only person that can decrypt it, since you have the private key. 
- This system also allows the receiver of a message know exactly who sent it. Consider that if I use my private key to encrypt a message and send it out, anyone with my public key can decrypt it. 
	- This has an important implication, which is that if you were able to successfully decrypt my message, then you know for a fact that I was the one who encrypted it. 
- See: Diffie-Helman

### Mail Poker Example
- *note:* is this an actual analogy of public key, or something else?
- [source](https://www.youtube.com/watch?v=mthPiiCS24A&list=PLt5AfwLFPxWLXe-ZqZyu0kSsaWd4FjXbj)
- Imagine we wanted to play a game over poker over the mail. Of course, each player needs to be assured that the same rules apply as if the game was in person (neither side knows the other's cards, each side only takes 5 cards, etc). 
- Player 1 puts each card in an envelope, and attaches a lock. This lock is only openable by player 1, but only one key is needed to open all the locks.
- Player 1 mails all 52 cards to Player 2. Player 2 then puts his own lock on all envelopes. Again, his key will open all envelopes. Then Player 2 will send the double-locked envelopes back to Player 1. (the randomized nature of the mail process acts as a secure and trusted method of shuffling) 
	- note: we have created a situation where neither player can reveal any card on their own. 
- Player 1 now has 52 double-locked envelopes, so now must choose 5 cards for Player 2. Once they are chosen, Player 1 takes his lock off those cards and mails just those 5 cards. Once Player 2 receives those cards, he can unlock them and now knows his hand. 
- Player 1 now has 47 double-locked envelopes, so now he must choose 5 for himself. Once he chooses, he sends them to Player 2. Player 2 removes his lock from those 5 cards, and sends them back to Player 1. Player 1 removes his lock, and now knows his own hand.

- This system of double-locking creates what is called *commutativity*. A commutative crypographic algorithm is one that is order-independent (ie. it doesn't matter which order the locks are taken off)  

#### Bridging the Analogy Gap
- If this game of poker were to take place over the internet, each of the 52 cards would have a numeric value (1-52). We need a way to "lock" each card. Player 1 creates his key (`k`) by choosing a random number. He then takes that number and raises each card to that value.
	- ex. if `k = 3`, then each card value is raised to the 3rd power. This is equivalent of locking the envelopes, as long as Player 2 doesn't know what `k` is.
- Player 2 then can create his own key (`j`), and raise each (already encrypted) card value to that value. Now we have double-locked cards.
	- It's important to note that `(2^5)^3` is mathematiacally equivalent to `(2^3)^5`. In other words, the locks can be applied or removed in any order. 
- Now, to remove the locks, each player simply needs to multiple the exponent by the inverse of their key (if Player 1 wants to remove the locks of 5 cards, then he just needs to multiply the exponent by `-3`)

* * *

Generating the keys
```sh
# generate RSA private key
$ openssl genrsa > private.pem

# derive public key
$ openssl rsa -in private.pem -pubout -out public.pem
```

* * *

*Sharing identical keys works fine among 2 people. What if Alice want to exchange stuff with another guy named Carl, and Alice doesn’t want anybody to see their stuff too? Alice can’t use the same lock and key that she shared with Bob, else Bob can unlock the box easily!  Of course Alice can share a completely new and different lock and key with Carl, but what if Alice wants to exchange stuff with 10 different people? She will need to keep and manage 10 different keys!  So Alice come out with a brilliant solution. Now, she only maintains one key (private key). She distribute the same padlocks (public key) to her friends. Anyone can close the padlocks (encrypt), but only she has the key to open (decrypt) them. Now, anyone can send stuff to Alice using the padlock she distributed, and Alice no longer have to manage different keys for different people.  If Alice wants to send something to Carl, she will ask for Carl’s padlock (public key) so that she can use it to lock (encrypt) her stuff and send it to Carl.  The basic principle is: everyone has their own private key to decrypt message, and they will provide senders their own public key for message encryption.*

## UE Resources
- [Keychain Access in Macos](https://www.youtube.com/watch?v=fdJKk89hTkw)
- [stackoverflow](https://superuser.com/questions/620121/what-is-the-difference-between-a-certificate-and-a-key-with-respect-to-ssl)