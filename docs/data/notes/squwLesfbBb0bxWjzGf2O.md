
Each SSH key pair has 2 components: public key and private key
- They are both created with `ssh-keygen` 

### Public key
- Public key is copied to the SSH server so it knows it can trust the user with the corresponding private key when it tries to connect via SSH
- Anyone that has the public key is able to encrypt the data in such a way that only the bearer of the private key can decrypt it. 
- Once an SSH server receives the public key from a user, it marks the key as trustworthy and is moved to the `authorized_keys` file.
- `ssh-copy-id` allows us to copy an SSH key from our local machine to a server to be used as an authorized key, allowing s the SSH client to login without password
	- What it does is it assembles a list of one or more fingerprints, and tries to log in with each key. It then makes a list of all the keys that failed login, and enables logins with those keys on the remote server (by adding them to the remote's `authorized_keys` file).
- Public keys are stored on SSH clients 

### Private key
- Considered proof of the user's identity, and only if the private key corresponds to the public key will the user be authenticated
- Because they are used for authentication, they are called identity keys
- The private key is stored on the SSH server
- When we open the private key, what we see is its encrypted form. To decrypt it, we need to enter the password that we created with `ssh-keygen`.
	- Therefore, without the password, the private key is useless.

* * *

## SSH Agent
ssh-agent stores unencrypted private keys in memory, allowing us to login without entering our password each time.
- Therefore, ssh-agent is a form of Single-Sign on (SSO)
- Without ssh-agent, upon entering the password, the decrypted key is stored in program memory (which is associated with a process (PID)). However, the SSH client process cannot be used to store the decrypted key, since the process is terminated once the remote login session has ended. This is why we use `ssh-agent`

`ssh-agent` works by creating a socket then checks for connections from ssh.
- anyone who is able to connect to this socket can also connect to the ssh-agent.
- upon starting, the agent creates a new directory in `/tmp` that determines permissions.

On most Linux systems, ssh-agent is automatically configured and run at login. 

On the server side, `PubkeyAuthentication` must be enabled in the `ssh_config` file to allow these key-based logins. 

we can use `ssh-add` tool to add identities to the agent.
- running `ssh-add` without args will add the default private keys to the agent (`~/.ssh/id_rsa`, `~/.ssh/id_dsa` etc.)
- `ssh-add -l` will list out the private keys accessible to the agent

### Agent Forwarding
Agent forwarding is a mechanism that gives an SSH server access to the SSH client's `ssh-agent`, allowing it to use it as if it were local. 
To use agent forwarding, the `ForwardAgent` option must be set to yes on the client 
- This allows us to execute commands on the server we SSHed into and have access to keys stored in `ssh-agent`. 
	- ex. if we are using keys to interact with Github (instead of entering password each time), we would be able to access them from the server. 
	- However, there is an implication of this, which is that we would not be able to run these commands as cron jobs on the server. Once our SSH connection ends, the the server no longer has access to `ssh-agent`, so it cannot use the private key.

## Debugging
- check client logs with `ssh -vvv <user>@<host>`

- check server logs at `/var/log/auth.log`

- on server, try editing `/etc/ssh/sshd_config`, changing: 
	1. `PasswordAuthentication yes`
	2. `PermitRootLogin yes`
	- restart sshd `service sshd restart`
	- on client, run `ssh-copy-id -i <remote-user>@<remote-ip>`
	- on host, revert the `sshd_config` options to their prior state

* * *

## Tunneling
- In order to tunnel, we need to set up a protocol. SSH is an example of such a protocol, where the receiver of the SSH command must have an SSH server running so that it may intercept the SSH message, and interpret it and enable the SSH connection. 

* * *

an OS can be preconfigured to enable SSH, so that it can be SHHed into the first time it is turned on.

* * *

## Config 
Client config (~/.ssh/config)
[good resource with lots of info](https://gravitational.com/blog/ssh-config/)
