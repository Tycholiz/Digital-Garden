
## What is it?
A JWT is simply a JSON payload containing a particular [[claim|auth.jwt.claim]]
- The key property of JWTs is that in order to confirm if they are valid we only need to look at the token itself (we don't have to contact a third-party service or keep it in-memory between requests).

When used as an authentication token, a JWT is a security token format that is used for exchanging authentication and authorization data between parties

The most common implementations of JWT that we will see are as [[access tokens|auth.tokens.access]] and [[refresh tokens|auth.tokens.refresh]]

The JWT should include the roles of the user, so that the backend knows what that user can do solely from the JWT itself.

a JWT is not encrypted. So any information that we put in the token is still readable to anyone who intercepts the token.
- therefore, we should never put anything in the JWT that a bad actor could leverage directly.

a JWT is actually Base64Url, not [[binary.encoding.base64]]
- this is virtually the same, except a couple characters are different to that it can exist as a URL param.
  - ex. `=` is displayed as `%3D`

### Construction
A JWT is made of 3 parts: the Header, the Payload and the Signature

#### Payload (claim)
![[auth.jwt.claim]]

#### Header
The receiver of the JWT needs to know what type of signature is used (here `RS256`).
```json
{
  "alg": "RS256",
  "typ": "JWT"
}
```

#### Signature
![[auth.jwt.signature]]

## Why use it?
The biggest advantage of JWTs (when compared to [[user session management|auth.session]] using an in-memory random token) is that they enable the delegation of the authentication logic to a third-party server, such as
- a centralized in-house auth server 
- LDAP (Lightweight Directory Access Protocol)
- third-party authentication provider like Auth0

This allows a system where:
- The external authentication server can be completely separate from our application server. There is no secret key that has to be shared over the network. This means all the application server has to do is check the JWT.
    - the authentication server has the private key, while the application server has the public key.
- No direct live link between the application server and the authentication server is needed
- The application server can be completely stateless (since we don't need to keep tokens in-memory between requests). The authentication server can issue the token, send it back and then immediately discard it.

The only way for an attacker to impersonate a user would be to either 
- steal both its username and personal login password 
- steal the secret signing key from the Authentication server.

## How do they work?
The receiver validates the content of the payload by inspecting the signature. 
- There are multiple types of signature, so one of the things that the receiver needs to know is for example which type of signature to look for.
    - this is found in the header

hashing the header and payload makes it much smaller. therefore hashing is not an functional part of it; it's a performance part.

### Analogy: Money vs Cheque
When we give a $20 bill to someone, there is not a second thought as to where this money came from or if the bearer is the legitimate owner. The person who we are giving this money to does not need to verify with anyone of its legitimacy, and it is simply trusted

On the other hand, when we pay with a cheque, a call needs to be made to the central authority (the bank) to verify if this cheque-holder is who they say they are.

In this analogy, the cash is like a JWT: the system is designed where we can just trust the bearer of the money.

* * *

### Token (ex. JWT)
- When user logs in, the server creates a JWT with the secret, and sends it to the client. The client stores the JWT in local storage, and includes that JWT in every request.
- The biggest difference here is that the user’s state is not stored on the server, as the state is stored inside the token on the client side instead

The whole point of JWTs is to not require centralized coordination.
- this is why there is no `/logout` endpoint to hit when we use JWTs for authentication. Instead, we just need to delete it from local storage.

A JWT is just a regular javascript object that is stringified, hashed, and cryptographically signed.
- Your token is signed with the secret, known only by the server. If someone changes the token on client side, it would fail validation and the server side framework would reject it. Therefore you can trust your token. Of course, the jwtSecret should be a secret only known by your authentication server and resource server.

JWTs are agnostic to what form of authentication you are using (ex. email, OAuth etc). Regardless, the response will contain the JWT.

You generate the token only if you trust the user who requested it.
- You trust the token as long as it has not expired and can be verified with the secret.
- The information in a JWT can be read by anyone, so do not put private information in a JWT. What makes JWTs secure is that unless they were signed by our secret, we can not accept the information inside the JWT as truth.

JWTs guarantee that the bearer of the token also owns the data that he is requesting.
- However JWTs don't guarantee encryption, which is why HTTPS is required. Otherwise, a man in the middle could take that server response (with the jwt) and use it to authenticate itself on your behalf, gaining access to all data.

JWTs come with a death sentence— that is, by nature they have an expiry date.
- this value is stored in the `iat` property. This can be thought of as the `date_of_death` property.
- The server determines the lifespan of the JWT, since it controls the expiry date. Therefore, JWTs give a uniform lifespan to all JWTs, but like God, it has control over ending your life prematurely in order to deny access.

JWTs are stateless (while sessions are stateful). This fact enables them to be verified on the server, without having to make a database call. This effectively makes them faster than using sessions (since sessions need to be stored)
- In reality, it's likely that the times we need to authenticate ourseleves with the JWT are also times that we need to interact with the database. This makes "saving trips to the database" more of a pipe-dream than a reality. At the end of the day, if we need to make a database interaction *and* authenticate ourselves, we are quicker just using a session rather than authenticating with a JWT (due to their size)

JWTs are huge. Storing a userid in a cookie is 6 bytes, while storing in the JWT (along with headers+secret) makes it 304 bytes.

![](/assets/images/2021-03-08-16-41-50.png)

- unlike a cookie, a JWT can contain an unlimited amount of data

### Logout
There is no `/logout` endpoint to hit, as all we need to do is delete the JWT kept on the client.
- This means the token is still valid even after you logout. This is why keeping a short expiry date is important

### Analogy
*"Pretend I’m blind and hard of hearing. Let’s also pretend that last week you bought me lunch, and now I need your bank account number to pay you back. If I ask you for your bank account number in person, and someone else shouts their bank account number, I might accidentally send them the money I owe you.
That’s because I heard someone shout a bank account number, and I trusted that it was you, even though in this case, it wasn’t.
JWTs were designed to prevent this sort of thing from happening. JWTs give people an easy way to pass data between each other, while at the same time verifying who created the data in the first place.
If I received 1,000,000 different JWTs that contained a bank account number, I’d easily be able to tell which one actually came from you.*"

## E Resources
- [Good high level overview. Includes links to other good content](https://blog.logrocket.com/jwt-authentication-best-practices/#:~:text=A%20JWT%20needs%20to%20be,storage%20(or%20session%20storage).)
- [Full guide](https://blog.angular-university.io/angular-jwt/)