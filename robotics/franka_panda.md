# Use Franka Panda

## Setup network
1. Check the `fci-ip` (fci-ip is the ip that you use to enter the desk control). In my use case, the `fci-ip` is `192.168.1.100`
2. Setup the control computer's network ip. This ip should be different only in the last part of the ip address from the `fci-ip`. In my case, set the control computer's network's ip address to `192.168.1.188`, netmask to `255.255.255.0`.
3. Disable network proxy

## Disable realtime requirement
`Robot` class construction method
```
Robot (const std::string &franka_address, RealtimeConfig realtime_config=RealtimeConfig::kEnforce, size_t log_size=50)
```

Set
```
realtime_config=RealtimeConfig::kIgnore
```
to ignore the requirement of realtime kernel.


 Reference: [Eric's reply](https://github.com/frankaemika/libfranka/issues/77#issuecomment-740316213)