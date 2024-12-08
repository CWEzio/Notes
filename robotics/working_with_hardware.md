# Network setup
I think the best way to connect with the robot is to use a router. In this way, your control computer and the robot are in the same subnet and they can find each other. Besides, the control computer can also access the internet via wire.

An example is:
- My robot has ip address `192.168.0.8`
- The router (gateway) has address `192.168.0.1`
- I should manually set the ipv4 to
    - Address: `192.168.0.*`
    - Netmask: `255.255.255.0`
        > The subnet mask 255.255.255.0 means that the first 24 bits (192.168.0.x) are for the network, and the last 8 bits (.x) are for the devices (hosts) within that subnet.
    - Gateway: `192.168.0.1`

