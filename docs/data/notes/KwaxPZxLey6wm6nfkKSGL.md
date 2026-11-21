
The `.netrc` file contains configuration and autologin information for the ftp client 
- It resides in the user's home directory

`.netrc` file takes the form:
> `remote-machine` `name`

The auto-login process searches the `.netrc` file for a machine token that matches the remote machine specified on the ftp command line
- Once a match is made, the subsequent `.netrc` tokens are processed, stopping when the end of file is reached or another machine or a default token is encountered

The following three lines must be included in the file. The lines must be separated by either white space (spaces, tabs, or newlines) or commas:

```
machine <remote-instance-of-labkey-server>
login <user-email>
password <user-password>
```
An example:
```
machine mymachine.labkey.org
login user@labkey.org
password mypassword
```
or:
```
machine mymachine.labkey.org login user@labkey.org password mypassword
```

### Using API Keys
When API Keys are enabled on your server, you can generate a specific token representing your login credentials on that server and use it in the netrc file. The "login" name used is "apikey" (instead of your email address) and the unique API key generated is used as the password

* * *

Note: the `.netrc` file only deals with connections at the machine level and should not include a port or protocol designation, meaning both "mymachine.labkey.org:8888" and "https://mymachine.labkey.org" are incorrect.
