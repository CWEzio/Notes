# Commandline Tools
## `grep`
### Search file content
Example:
```
grep -nr --include="\*.h" -A2 "AddContactMaterial" ~/drake 
```
- flags:
    - `-n`: Show relative line number in the file.
    - `-r`: Recursively search subdirectories listed.
    - `--include="\*.h"`: all `.h` files (escape with \ to avoid terminal autocompletion).
    - `--exclude`: exclude files
    - `--exclude-dir=build,node_modules`: exclude the `build` and `node_modules` directories. Use `,` to seperate two directories.
    - `-A`: ahead, also show lines after the search pattern (the search pattern is ahead by *N*).
    - `-B`: below, also show lines before the search pattern (the search pattern is below by *N*).
    - `-C`: context, include both lines after and below the search pattern by *N*.
    - Use `-i` to make `grep` case insensitive.

One useful usage case for above is to use `grep` to search for the header file to include.

Suppose that I find `AddContactMaterial` function from the documentation, I can determine which header file to include by using the powerful grep:
```
grep -nr --include \*.h -A2 "AddContactMaterial" ~/drake 
```
The output would be like:
```c++
/home/chenwang/drake/geometry/proximity_properties.h:94: * AddContactMaterial() adds general contact material properties to the given
/home/chenwang/drake/geometry/proximity_properties.h-95- * set of proximity `properties`. These are the properties required by the
/home/chenwang/drake/geometry/proximity_properties.h-96- * default point contact model. However, other contact models can opt to use
--
/home/chenwang/drake/geometry/proximity_properties.h:107:void AddContactMaterial(
/home/chenwang/drake/geometry/proximity_properties.h-108-    const std::optional<double>& dissipation,
/home/chenwang/drake/geometry/proximity_properties.h-109-    const std::optional<double>& point_stiffness,
--
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h:283:- AddContactMaterial()
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-284-- AddRigidHydroelasticProperties()
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-285-- AddCompliantHydroelasticProperties()
--
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h:290:AddContactMaterial() isn’t hydroelastic contact specific, but does provide a
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-291-mechanism for setting the friction coefficients that hydroelastic and point
/home/chenwang/drake/multibody/hydroelastics/hydroelastic_user_guide_doxygen.h-292-contact models both use.
--
/home/chenwang/drake/multibody/plant/multibody_plant.h:1452:  ///   the function geometry::AddContactMaterial(), or
/home/chenwang/drake/multibody/plant/multibody_plant.h-1453-  /// - define it in an input URDF/SDFormat file as detailed here:
/home/chenwang/drake/multibody/plant/multibody_plant.h-1454-  ///   @ref tag_drake_hunt_crossley_dissipation.
```
Then I know that `AddContactMaterial` is defined in `proximity_properties.h`.

## `find`
Check (`the missing semester`)[https://missing.csail.mit.edu/2020/shell-tools/]

```python
# Find all directories named src
find . -name src -type d
# Find all python files that have a folder named test in their path
find . -path '*/test/*.py' -type f
# Find all files modified in the last day
find . -mtime -1
# Find all zip files with size in range 500k to 10M
find . -size +500k -size -10M -name '*.tar.gz'
```

## `apt`
1. Use `apt search *keyword*` to search for available packages. This is very useful if you are not sure about the full name of the package.

## `mv`
- `mv` does not show progress. Consider use `rsync`

## `rsync`
TODO: Add notes

## `ffmpeg`
### cut a video
Use command 
```
ffmpeg -ss 00:01:00 -to 00:02:00  -i input.mp4 -c copy output.mp4
```
Check George Chalhoub's [answer](https://stackoverflow.com/a/42827058/12825127) for detail.

## `sed`
`sed` is a powerful tool for text manipulation.

- Substitute the first occurence of `old_text` with `new_text` in each line.
    ```
    sed 's/old_text/new_text/' <filename>
    ```
    - `s/old_text/new_text/g` for global sustitution.
    - `-i` flag for modification in place. Or the original file won't be modified. Instead, the result of substitution will be directed to the standard output.

- Delete lines matching a pattern
    ```
    sed '/pattern/d' filename
    ```

## `tar`


# Apps
## clashy
### Edit Profile
- Note that the profiles are case-sensitive. One example is that you should use `DIRECT` consistently. Inconsistent usage like `direct` will cause errors.


# How-to-do
## How to enable and disable `wifi` through the command line
```
sudo nmcli radio wifi off
sudo nmcli radio wifi on
```
Alternative choice is to use
```
sudo nmtui
```
which offers a GUI in the terminal. More intuitive.

Other related things:
- Show wifi list
    ```
    nmcli d wifi list
    ```

## Purge Nvidia Driver
Find [this answer](https://askubuntu.com/a/206289/1004561) quiet useful. In short,
```
sudo apt-get remove --purge '^nvidia-.*'
sudo apt-get install ubuntu-desktop
sudo rm /etc/X11/xorg.conf
echo 'nouveau' | sudo tee -a /etc/modules
```
Not all above commands are needed, but it does not hurt to run them all.

## How to mount a disk partition into a folder
TODO: complete this

Use `fstab`. Check [this conversation](https://chatgpt.com/share/671b9c9a-adf8-8004-9d34-f56df6d31401) with ChatGPT for detail.

# Bugs 
## Cannot open terminal with ctrl+alt+t
At first, I think the problem is that the locale configuration is wrong. However, after reconfiguring the locale with every method I have found on the internet, this problem persists.

I note https://askubuntu.com/questions/1040566/cant-open-terminal-in-ubuntu-18-04-after-upgrade-from-17-10.

It turns out that the issue is because I have set the default python3 to python3.7. 

The solution is
```console
code /usr/bin/gnome-terminal
```
 The first line is 
 ```
#! /usr/bin/python3
 ```
 Change it to 
 ```
 #! /usr/bin/python3.6 
 ```
Works like magic!


## NewsFlash Cannot show browse content
Run newsflash with 
```
flatpak run io.gitlab.news_flash.NewsFlash
```
The opened flatpak does not show the browse content. And there are error:
```
KMS: DRM_IOCTL_MODE_CREATE_DUMB failed: Permission denied
Failed to create GBM buffer of size 714x807: Permission denied
```
I find solution from [this github issue](https://github.com/johnfactotum/foliate/issues/1093?ref=news.itsfoss.com). Setting `WEBKIT_DISABLE_COMPOSITING_MODE=1` or `WEBKIT_DISABLE_DMABUF_RENDERER=1` fixes the problem.
Just run newsflash with 
```
WEBKIT_DISABLE_DMABUF_RENDERER=1 flatpak run io.gitlab.news_flash.NewsFlash
```

## Missing dependencies for SOCKS support
Currently, I am using electron-ssr. However, this gives me an exception 
```
requests.exceptions.InvalidSchema: Missing dependencies for SOCKS support.      

```
I solve this by 
```console
export all_proxy="http://127.0.0.1:12333"
```
Here http://127.0.0.1:12333 is my ssr port address.

## Unable to fetch gpg key
Encounter
```
Executing: /tmp/apt-key-gpghome.PpjCyyBi8S/gpg.1.sh -q --fetch-keys https://bazel.build/bazel-release.pub.gpg
gpg: WARNING: unable to fetch URI https://bazel.build/bazel-release.pub.gpg: Connection timed out
```
when install the `manipulation` course supplement's prerequiste using the provided shell script `install_prereqs.sh`.

Solution:
This is because of the GFW. The apt-key cannot get the key. Need to first manully download the key from https://bazel.build/bazel-release.pub.gpg. Then run 
```zsh
sudo apt-key add key.gpg
```
suppose the key file is named `key.gpg`.

Or one can use a single command to do the trick:
```zsh
curl -sSL \
'https://bazel.build/bazel-release.pub.gpg' \
| sudo apt-key add -
```

## `zsh-vi-mode`: Missing letters after copy-paste in normal mode 
Pasting in normal mode can cause missing letters, like miss the *g* letter. The reason behind this is complex, and I am not very clear. However, I suspect the reason is that the `zsh-vi-mode` does not handle the [bracketed-paste](https://en.wikipedia.org/wiki/Bracketed-paste) well.

Before pasting, make sure you're in insert mode. Press `i` to enter insert mode, and then paste the text. The paste works well in the insert mode.

## Bluetooth headphone no voice 
In `Ubuntu 20.04` (also earlier version), I often encounter this issue. The headphone will have no voice after a while of unattendence, or reconnection. To deal with this problem, I can restart the computer, but this method is not convenient.

Though not tidy, but the following command works:
```
sudo /etc/init.d/bluetooth restart
```

Some also suggest to use the following command 
```
pulseaudio -k
```
In my experience, this command works, but it makes the sound not clear.

## F1 to F12 does not work
I have bought a new wireless keyboard and I find that the `F1` to `F12` keys do not work normally in Ubuntu. It turns out that the problem is that this keyboard use the `hid_apple` driver, and in the default mode the `Fn` key is always on. 

To change this behavior, 

1. Append the configuration line
    ```
    echo options hid_apple fnmode=2 | sudo tee -a /etc/modprobe.d/hid_apple.conf
    ```

2. 
    ```
    sudo update-initramfs -u -k all
    ```

3. Reboot the computer

Regarding the `fnmode` option:
- `0`: Fn key disabled
- `1`: Fn key pressed by default
- `2`: Fn key released by default

