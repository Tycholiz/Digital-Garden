
### Check which DNS server is configured for the device
Mac
`scutil --dns | grep 'nameserver\[[0-9]*\]'`

Linux
`cat /etc/resolv.conf`
