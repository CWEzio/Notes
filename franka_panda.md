# Use Franka Panda

## Setup network
1. Check the `fci-ip` (fci-ip is the ip that you use to enter the desk control). In my use case, the `fci-ip` is `192.168.1.100`
2. Setup the control computer's network ip. This ip should be different only in the last part of the ip address from the `fci-ip`. In my case, set the control computer's network's ip address to `192.168.1.188`, netmask to `255.255.255.0`.