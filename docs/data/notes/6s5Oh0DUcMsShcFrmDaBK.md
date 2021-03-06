
A JWT is a security token format that is used for exchanging authentication and authorization data between parties
The most common implementations of JWT that we will see are as [[access tokens|auth.jwt.access-token]] and [[refresh tokens|auth.jwt.refresh-token]]

### Token (ex. JWT)
- When user logs in, the server creates a JWT with the secret, and sends it to the client. The client stores the JWT in local storage, and includes that JWT in every request.
- The biggest difference here is that the user’s state is not stored on the server, as the state is stored inside the token on the client side instead

- the whole point of JWTs is to not require centralised coordination
	- this is why there is no `/logout` endpoint to hit. 
- A JWT is just a regular javascript object that is stringified, hashed, and cryptographically signed.
- Your token is signed with the secret, known only by the server. If someone changes the token on client side, it would fail validation and the server side framework would reject it. Therefore you can trust your token. Of course, the jwtSecret should be a secret only known by your authentication server and resource server.
- JWTs are agnostic to what form of authentication you are using (ex. email, OAuth etc). Regardless, the response will contain the JWT.
- You generate the token only if you trust the user who requested it.
- You trust the token as long as it has not expired and can be verified with the secret.
- The information in a JWT can be read by anyone, so do not put private information in a JWT. What makes JWTs secure is that unless they were signed by our secret, we can not accept the information inside the JWT as truth.
- JWTs guarantee that the bearer of the token also owns the data that he is requesting.
	- However JWTs don't guarantee encryption, which is why HTTPS is required. Otherwise, a man in the middle could take that server response (with the jwt) and use it to authenticate itself on your behalf, gaining access to all data.
- JWTs come with a death sentence— that is, by nature they have an expiry date.
	- this value is stored in the `iat` property. This can be thought of as the `date_of_death` property.
	- The server determines the lifespan of the JWT, since it controls the expiry date. Therefore, JWTs give a uniform lifespan to all JWTs, but like God, it has control over ending your life prematurely in order to deny access.
- JWTs are stateless (while sessions are stateful). This fact enables them to be verified on the server, without having to make a database call. This effectively makes them faster than using sessions (since sessions need to be stored)
	- In reality, it's likely that the times we need to authenticate ourseleves with the JWT are also times that we need to interact with the database. This makes "saving trips to the database" more of a pipe-dream than a reality. At the end of the day, if we need to make a database interaction *and* authenticate ourselves, we are quicker just using a session rather than authenticating with a JWT (due to their size)
- JWTs are huge. Storing a userid in a cookie is 6 bytes, while storing in the JWT (along with headers+secret) makes it 304 bytes.

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

# E Resources
[Good high level overview. Includes links to other good content](https://blog.logrocket.com/jwt-authentication-best-practices/#:~:text=A%20JWT%20needs%20to%20be,storage%20(or%20session%20storage).)
