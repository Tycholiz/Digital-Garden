
Notice that the structure of the DNS database is similar to the structure of a UNIX filesystem.
- In the DNS database, the root node is written as a dot (`.`). In the Unix filesystem, the root is written as a slash (`/`).

Each partition of the database is known as a [[zone|dns.db.zone]] (a.k.a *namespace*)

Each subtree represents a partition of the overall database— a directory in the Unix filesystem, or a *domain* in the Domain Name System.
- each domain can be further partitioned into *subdomains*

In a filesystem, directories contain files and subdirectories. Likewise, domains can contain both hosts and subdomains.

A full domain name identifies its position in the database, similar to how an absolute path specifies its place in the filesystem.


![](/assets/images/2022-03-12-15-46-04.png)
