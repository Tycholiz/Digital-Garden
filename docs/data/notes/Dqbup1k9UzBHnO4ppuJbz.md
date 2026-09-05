
### Systemd
- systemd is a collection of programs that provides system components for Linux.
- Its purpose is to unify system configuration across different Linux distributions.
- Systemd's primary component is a system and service manager, which is an init (boot) system used to manage user processes.
	- systemd also provides replacements for various daemons and utilities of the Linux system, including device management, login management, network connection management.
- Effectively, systemd is used on most Linux systems, and has replaced distribution-specific init systems. 
- below is the systemd startup log.
![74a24a20baffe9c81ed4a14a5d4c398a.png](:/3d7b66b1422e473096a6481247f59393)

- `systemctl` is a utility used to control systemd 
	- ex. we can issue a command to restart the `ssh` server
