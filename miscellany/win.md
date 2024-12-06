# WSL
## Visit Windows files from WSL
```
cd /mnt/<path-to-windows-file>
```

## wsl: 检测到 localhost 代理配置，但未镜像到 WSL。NAT 模式下的 WSL 不支持 localhost 代理
Create `.wslconfig` file under `C:\%USERPROFILE%\.wslconfig` (`%USERPROFILE%` is the current user directory, like `c/Users/chen`).
Include the following in `.wslconfig`:
```
[experimental]
autoMemoryReclaim=gradual  # gradual  | dropcache | disabled
networkingMode=mirrored
dnsTunneling=true
firewall=true
autoProxy=true
```

The above solution is from [this issue](https://github.com/microsoft/WSL/issues/10753).

## Proxy

Add the following to `.zshrc` file
```zsh
export hostip=$(cat /etc/resolv.conf | grep "nameserver" | cut -f 2 -d " ")

set_proxy(){
    export https_proxy=http://$hostip:7078
    export http_proxy=http://$hostip:7078
    export telnet_proxy=http://$hostip:7078
    export ftp_proxy=http://$hostip:7078
}
unset_proxy(){
    unset https_proxy
    unset http_proxy
    unset telnet_proxy
    unset ftp_proxy
}
```
> Remember to turn on `allow LAN` in the clash/monocloud client.

# PowerToys
PowerToys comes with a lot of useful tools.
- `Keyboard Manager`
    - Remap keys.
- `PowerToys Run`
    - `alt + space` to call it.
    - Search and open applications; Search web; Calculator ...

# Problems
## OneDrive cannot sync
Onedrive does not support proxy because of the authentication issue. Please turn off the system proxy.
