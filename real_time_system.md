# Install real time kernel on Ubuntu 22.04
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

# Install real time kernel on Ubuntu 20.04.4 LTS 
Install the real time kernel on Ubuntu 20.04 is more complicated, you have to build the realtime kernel yourself. I maily follow the instruction from the [`Libfranka` documentation](https://frankaemika.github.io/docs/installation_linux.html), with some necessary modifications to make things work.
The steps is as follows:
1. Install all the necessary dependencies
```
sudo apt-get install build-essential bc curl ca-certificates gnupg2 libssl-dev lsb-release libelf-dev bison flex dwarves zstd libncurses-dev
```
2. Make a directory to store the kernel files
```
cd ~
mkdir realtime_patch
cd realtime_patch
```
3. Downnload the kernel from [linux kernal release page](https://mirrors.edge.kernel.org/pub/linux/kernel/v5.x/). The version that I use is `linux-5.15.10.tar.xz` <br>
Alternatively, you can use `curl`
```
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.15.10.tar.xz
curl -SLO https://www.kernel.org/pub/linux/kernel/v5.x/linux-5.15.10.tar.sign
```
4. Download the realtime patch from [realtime patch release page](https://mirrors.edge.kernel.org/pub/linux/kernel/projects/rt/). Select the corresponding version with the kernel. The version that I use is `patch-5.15.10-rt24.patch.xz` <br>
Alternatively, you can use `curl`
```
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/4.14/older/patch-5.15.10-rt24.patch.xz
curl -SLO https://www.kernel.org/pub/linux/kernel/projects/rt/4.14/older/patch-5.15.10-rt24.patch.sign
```
5. Decompress the files with 
```
xz -d *.xz
```
> Note that I skip the `verifying file integrity` part.
6. Extract kernel source file and apply the patch with
```
tar xf linux-*.tar
cd linux-*/
patch -p1 < ../patch-*.patch
```
7. Copy your currently booted kernel configuration as the default config for the new real time kernel
```
cp -v /boot/config-$(uname -r) .config
```
8. Use this config as default with
```
make olddefconfig
```
9. Make modification with the config
```
make menuconfig
```
You will enter a terminal interface. Make the following modification.
- Navigate with the arrow keys to General Setup > Preemption Model and select Fully Preemptible Kernel (Real-Time).
- Navigate to Cryptographic API > Certificates for signature checking (at the very bottom of the list) > Provide system-wide ring of trusted keys > Additional X.509 keys for default system keyring  
- Remove the “debian/canonical-certs.pem” from the prompt and press Ok. 
> The following the not mentioned in `libfranka` documentation. I find this from `nekoliten`'s answer in [this page](https://gitlab.com/CalcProgrammer1/OpenRGB/-/issues/950). This step is key to build the `deb` package successfully.
- Again in the Cryptographic API > Certificates for signature checking tab, find > Provide system-wide ring of revocation certificates > X.509 certificates to be preloaded into the system blacklist keyring
- Remove the “debian/canonical-certs.pem” from the prompt and press Ok. 
- Save the configuration to `.config` and leave the `TUI`.

10. Compile the kernel 
```
make -j$(nproc) deb-pkg
```
11. Install the new created package
```
sudo dpkg -i ../linux-headers-*.deb ../linux-image-*.deb
```

> The followings are settings for using `libfranka`
12. Allow a user to set real-time permissions for its processes
```
sudo addgroup realtime
sudo usermod -a -G realtime $(whoami)
```
13. Add the following limits to the *realtime* group in `/etc/security/limits.conf`
```
@realtime soft rtprio 99
@realtime soft priority 99
@realtime soft memlock 102400
@realtime hard rtprio 99
@realtime hard priority 99
@realtime hard memlock 102400
```





## Change default grub menu selection
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

## How to run your own process with realtime priority?
Use 
```zsh
sudo chrt 99 myprog
```