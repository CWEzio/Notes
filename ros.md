# My learning note on ROS

## Problems 
### `rosrun` cannot find my python script
It turns out that rosrun can automatic detect executable files. However, at first I need to make the script executable by `chmod +x` and add 
```python
#!/usr/bin/env python3
```
on the first line of the python script.