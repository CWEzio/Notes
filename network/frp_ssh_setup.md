# Setup

## Server A (server with public ip, like aliyun)
Preparation
- Open tcp port `6000` and `7000`

Install `frp`
- `wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz`
- `tar zxvf frp_0.61.1_linux_amd64.tar.gz`
- `cd frp_0.61.1_linux_amd64`
- Modify `frps.toml`
    ```toml
    # frps.toml
    bindPort = 7000
    ```
  > Above is the default content of `frps.toml` so this step can be skipped.

Add `frps` to service
- `sudo vim /etc/systemd/system/frps.service`
- In `frps.service`, add the following
    ```toml
    [Unit]
    Description=frps service
    After=network.target

    [Service]
    Type=simple
    User=root
    WorkingDirectory=/root/frp/frp_0.61.1_linux_amd64
    ExecStart=/root/frp/frp_0.61.1_linux_amd64/frps -c /root/frp/frp_0.61.1_linux_amd64/frps.toml
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target
    ```
   > - Modify  `/root/frp/frp_0.61.1_linux_amd64` to the directory where you install frp
   > - Modify `root` to your user name
- `sudo chmod 644 /etc/systemd/system/frps.service`
- Reload systemd to recognize the new service
    ```
    sudo systemctl daemon-reload
    ```
- Enable the service to start at boot:
    ```
    sudo systemctl enable frps
    ```
- `sudo systemctl start frps`
- You can check the status of the frps service with
    ```
    sudo systemctl status frps
    ```

## Server B (Computer to ssh into)
Install `frp`
- `wget https://github.com/fatedier/frp/releases/download/v0.61.1/frp_0.61.1_linux_amd64.tar.gz`
- `tar zxvf frp_0.61.1_linux_amd64.tar.gz`
- `cd frp_0.61.1_linux_amd64`
- Modify `frpc.toml`
    ```toml
    serverAddr = "server-ip"
    serverPort = 7000

    [[proxies]]
    name = "ssh"
    type = "tcp"
    localIP = "127.0.0.1"
    localPort = 22
    remotePort = 6000
    ```
    > modify `server-ip` to your server's ip

Add `frpc` to service
- `sudoedit /etc/systemd/system/frpc.service`
- Add the following to `frpc.service`:
    ```toml
    [Unit]
    Description=frpc service
    After=network.target

    [Service]
    Type=simple
    User=chenwang
    WorkingDirectory=/home/chenwang/frp_0.61.1_linux_amd64/
    ExecStart=/home/chenwang/frp_0.61.1_linux_amd64/frpc -c /home/chenwang/frp_0.61.1_linux_amd64/frpc.toml
    Restart=always
    RestartSec=5

    [Install]
    WantedBy=multi-user.target
    ```
    > - Modify `/home/chenwang/frp_0.61.1_linux_amd64/` to the directory of your `frp`
    > - Modify `User` accordingly
- `sudo chmod 644 /etc/systemd/system/frpc.service`
- `sudo systemctl daemon-reload`
- `sudo systemctl enable frpc`
- `sudo systemctl start frpc`

## ssh to server B
- `ssh -p 6000 chenwang@server-ip`
    > Remember to change `server-ip` accordingly.

