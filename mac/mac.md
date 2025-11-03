# Keyboard shortcut
- cut a file
    - `ctrl + c` to select the file to copy and `option + command + v` to paste the file and delete the original file.

## Desktop and Window Control
### Desktop Control
- `ctrl + →/←` to move to right or left desktop.
    - For the active display (where the mouse is on)

### Window Control
- I find [this reddit post](https://www.reddit.com/r/MacOS/comments/ra7jn1/comment/hngr92m/?utm_source=share&utm_medium=web3x&utm_name=web3xcss&utm_term=1&utm_content=share_button) explains the rule of minimize and hide quiet well. Here is the original post:
    > Command+Tab, Command+H, and Command+Option+H should be your most-keyboard shortcuts. Minimize is rarely used.
    > Use Hide whenever you intend to switch to another app, similar to Command Tab. When you return to the app, all of its windows will be restored just as you left them.
    > 
    > For example, if you're working in Word and you want to get to Safari underneath, hit Command H. That will hide Word and activate Safari. Command+Tab will take you back to Word.
    > 
    > Or, say you're in Word and you get a new email. Command Tab to the Mail app to read the mail, then Command H to hide Mail and go back to Word.
    > 
    > Minimize a window only when you want to manage the open windows or documents inside of a multi-document app. For example, say you have five Word documents open and you only want to work with two of them. Minimize the other three, and that will take them off screen and out of the Command+` (back tick) loop.
    > 
    > You never minimize window when you're trying to get to another app. E.g., You would never minimize a Word document to try to see a Safari window underneath.
- `command + h`: hide the windows of the front app.
- `option + command + h`: hide the windows except the front app.
- `command + tab`: cycle through Apps. 
    - This will open all the hidden windows of the app, but not the minimized windows. If all the windows of the switched app are hidden, after `tab` to the switched app, press `option`, then release `command`. This will bring the last minimized window un-minimized, though in practice this is seldomly needed (if all windows are minimized, why not use hide?).
- ```command + ` ``` to cycle through all opened windows. Minimized windows are removed from this cycle list.
- `ctrl + ↓`: show all windows of the current app.
  - After all windows are displayed, you can select the minimized window to bring them un-minimized.
- `command + m` to minimize the front window to the dock.



# Problems 
## Use an external keyboard (MCHOSE)
- Use [`Karabiner`](https://karabiner-elements.pqrs.org/)

## Mouse use natural scroll (reverse scroll)
- Use `Karabiner` to reverse the mouse wheel vertical direction.
    - MacOS's setting will change the behavior for both the mouse and the trackpad, which is not desired.

## Apple could not verify “xxx” is free of malware that may harm your Mac or compromise your privacy.
- Make sure this APP is safe.
- Go to `System Setting` > `Privacy & Security`
- Scroll down and find the message
    ```
    xxx was blocked because it is not from an identified developer.
    ```
- Click `Open Anyway`.

