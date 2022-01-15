
# Password Cracking
- with `hashcat`, dedicated servers for cracking passwords can hash a word and check for the presence of the hash in a big list of hashes 40 billion times per second (if the hashes were hased with md5 algo). If a more secure algo is used like SHA512 or Bcrypt, it would be in the thousands instead. 
- In a brute force attack, a 7 letter word all lower-case has 26^7 possible combinations, which is just over 8 billion combinations. This makes an md5 stored 7 letter word breakable in less than a second.
