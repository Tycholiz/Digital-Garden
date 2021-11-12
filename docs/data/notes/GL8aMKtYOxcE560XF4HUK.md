
Security by obscurity is generally not a thing, since DigitalOcean and AWS etc have known IP ranges that all hackers always target; if you don't ban them they'll eventually brute force you
- Use ssh keys from day 1 and/or install fail2ban (preferably both)
    - instead of fail2ban, you can whitelist only your CDN's IP addresses (like Cloudflare), through the use of DigitalOcean firewalls (or something similar). That way, you can’t get DoS’d because everything has to go the CDN route.