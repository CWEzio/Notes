# Sharp bits
- The `DISPLAY` variable is not automatically set in the ssh session.

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