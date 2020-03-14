# Bug and Solution

## Cannot open terminal with ctrl+alt+t
At first I think the problem is locale configuration is wrong. However, after reconfigure locale with every methods I have found from internet, this problem persist.

I note https://askubuntu.com/questions/1040566/cant-open-terminal-in-ubuntu-18-04-after-upgrade-from-17-10.

It turns out that the issue is because I have set the default python3 to be python3.7. 

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