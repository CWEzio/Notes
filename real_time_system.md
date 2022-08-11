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

## Change default kernel
The above operation will make the realtime kernel the deault kernel. However, since the `nvidia-driver` does not work in the realtime kernal. We would like to choose the generic kernel as the default one.
Refer to [this website](https://www.how2shout.com/linux/how-to-change-default-kernel-in-ubuntu-22-04-20-04-lts/#:~:text=Save%20the%20file%20Ctrl%2BO,then%20exit%20it%20Ctrl%2BX.&text=And%20as%20you%20start%20your,default%20one%20on%20your%20system.) and [this answer](https://unix.stackexchange.com/a/421650) for more detail.

In short, add the following two lines to `/etc/default/grub`.
```
GRUB_SAVEDEFAULT=true
GRUB_DEFAULT=saved
```
Then, 
```
sudo update-grub
```
Then, the choice you choose in the grub menu will become the default. When you manually choose a different entry, that becomes the new default.