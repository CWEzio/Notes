# Sharp bits
- The `DISPLAY` variable is not automatically set in the ssh session.

# Usages
## Set a remote connection in `~/.ssh/config`
Sample:
```bash
Host lab
  HostName 47.237.21.54
  User chenwang
  Port 6000
  ProxyCommand nc -X connect -x 127.0.0.1:7890 %h %p # use proxy 127.0.0.1:7890
```
Then you can connect to this host with `ssh lab`.

# Memo
- The authorized keys are stored in `~/.ssh/authorized_keys`.
- The connection aliases are stored in `~/.ssh/config`. 
  - An example of the connection alias:
    ```
    Host lab
        HostName 47.239.141.59
        User chenwang
        Port 6000
    ``` 