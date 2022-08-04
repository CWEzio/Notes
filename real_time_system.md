# Install real time system on Ubuntu 22.04
As Ubuntu 22.04 LTS beta is released (note that this is a commercial project, and only limited usage for free account), installing real-time kernel becomes easier. You can install it in the following steps. 

1. Get a ubuntu advantage account and get a free subscription token (up to 3 machines)
2. attach the machine to a UA subscription
```
ua attach <free token>  \\ your token here
```
3. upgrade the ubuntu-advantage-tools
```
sudo apt install ubuntu-advantage-tools=27.9~22.04.1
```
4. Enable the real-time kernel 
```
ua enable realtime-kernel --beta
```