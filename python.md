
# Bugs
## `Autopep8` works too aggressivly 
In `vscode`, I use `autopep8` to format my code. However, it works too aggressively and it change my import order.
I use 
```python
import sys  
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  
```
to add python path. However, `autopep8` move this to the end of the import which breaks everything.

The solution is to add `# nopep8` at the end of the line to told `autopep8` not to format this line.

For example,
```python
import sys  # nopep8
sys.path.append('/home/chenwang/catkin_ws/src/turtlebot_tmpc/script')  # nopep8
```