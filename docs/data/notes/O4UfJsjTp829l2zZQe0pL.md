
### Signature
- created using the header, the claims AND the secret. Therefore, this unique combination creates a hash, and if something in the claim were to change, then the signature would be different and would no longer match up. 
	- What this means is that if the 1st and 2nd set of the JWT don't change, than neither will the 3rd (of course assuming the secret remains unchanging)
		- in practice jwt will always be different between sign ins of the same user, since the `exp` variable will always be different
![8ded9f2b6239d7464fa7e85badce77c6.png](:/fa5c3e7e2d95477e85f51bed49c6b4d9)
