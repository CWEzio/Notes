
# Set up a self-host server
Reference:
- [this article](https://developer.aliyun.com/article/1299504).

## Set up aliyun
1. Create ECS
2. Reset ECS password
3. 在安全组中，允许端口访问(all IP)
    - 自定义tcp 21115/21119
    - 自定义UDP 21116
> After setting 端口访问, restart the ECS.

## Set up rustdesk server on ECS/VPS
1. `mkdir -p /root/rustdesk`
2. `touch /root/rustdesk/docker-compose.yml`
3. Add the following to the config file
    ```
    version: '3'

    networks:
    rustdesk-net:
        external: false

    services:
    hbbs:
        container_name: hbbs
        ports:
        - 21115:21115
        - 21116:21116
        - 21116:21116/udp
        - 21118:21118
        image: rustdesk/rustdesk-server:1.1.14
        command: hbbs -r 这里替换成你服务器的公网ip:21117
        volumes:
        - ./data:/root
        networks:
        - rustdesk-net
        depends_on:
        - hbbr
        restart: unless-stopped

    hbbr:
        container_name: hbbr
        ports:
        - 21117:21117
        - 21119:21119
        image: rustdesk/rustdesk-server:1.1.14
        command: hbbr
        volumes:
        - ./data:/root
        networks:
        - rustdesk-net
        restart: unless-stopped
    ```
    > Remember to modify this line 
    > ```
    >     command: hbbs -r 这里替换成你服务器的公网ip:21117
    > ```
4. `cd /root/rustdesk`
5. Start the rustdesk server container with `docker compose up -d`

Notes
- The key is stored in `/root/rustdesk/data/id_ed25519.pub`.
- Check logs with `docker compose logs hbbr` and `docker compose logs hbbs`.

## Set on client
- Got to `Setting` → `Network`. Set the `ID/Relay server` accordingly.
- Optionally, also set the `Socks5/Http(s) Proxy`. This can be useful when the private server is outside of mainland China and your client is inside mainland.

# Problem Shooting
## Cannot connect to other computer, even when both computers are ready
With my new home WiFi, I cannot connect to the remote computer. Both two computers are ready. With investigation, I found that the cause might be that the WiFi network blocking the ports rustdesk uses. Setting the proxy for the rustdesk solves the problem.
<img src="./asset/rustdesk/2025-08-18-15-34-31.png" width=300 />
