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


# Problems
## Onedrive cannot sync
Onedrive does not support proxy because of the authentication issue. Please turn off the system proxy.
