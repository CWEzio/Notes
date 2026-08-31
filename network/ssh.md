# Sharp bits
- The `DISPLAY` variable is not automatically set in the ssh session.

# Usages
## Use proxy
Sample:
```bash
Host lab
  HostName 47.xxx.21.54
  User chenwang
  Port 6000
  ProxyCommand nc -X connect -x 127.0.0.1:7890 %h %p # use proxy 127.0.0.1:7890
```

## ssh to a computer via bridge
Suppose I have a h20 server in the lab, a lab computer, and my laptop. The h20 server can only be accessed by device in the local network (the lab computer). To access the h20 server from my laptop, I can add the following config:
```bash
Host h20
  HostName xxx.16.120.9
  User xxx
  ProxyJump lab
  ProxyCommand None
```

Then I can assess the h20 server via `ssh h20`.

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