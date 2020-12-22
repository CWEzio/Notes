## Add conda virtual enviornment to jupyter notebook

* create a new env
  ```
  conda create -n mr_env python=3.7    
  ```
* install ipykernel to this env
  ```
  conda install -c anaconda ipykernel
  ```
* add this env to jupyter notebook
  ```
  python -m ipykernel install --user --name=firstEnv
  ```
* deactivate the env
  ```
  conda deactivate
  ```
Or simply follow this tutorial
https://janakiev.com/blog/jupyter-virtual-envs/
It contains more information.

## Write python module

Refer to np8's answer
https://stackoverflow.com/questions/714063/importing-modules-from-parent-folder
One thing to note is that setup.py need to be above the main folder.

## View Eigen Value when using clion debug(gdb)

Use drake_gdb https://github.com/SeanCurtis-TRI/drake_gdb

* git clone https://github.com/SeanCurtis-TRI/drake_gdb

* Create the file ~/.gdbinit

* insert the following lines into .gdbinit
  ```
  python
  import sys
  sys.path.append("path of your drake_gdb")
  import drake_gdb 
  drake_gdb.register_printers()
  end
  ```

One thing to note is that I encounter bug when use the author's install instruction. It seems that gdb use /usr/bin/python which is python2.7. 
```
sys.path.insert(0, "$DRAKE_GDB_ROOT$")
```
given by the authors' instruction seems does not work. import drake_gdb will give
```python
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
ImportError: No module named drake_gdb
```
Change this line to 
```
sys.path.append("path of your drake_gdb")
```
will solve this problem. Also use $DRAKE_GDB_ROOT is not necessary.

## Mujoco convert urdf to xml
Some useful reference.
https://zhuanlan.zhihu.com/p/99991106
http://www.mujoco.org/forum/index.php?threads/meshes-ignored-when-converting-urdf-to-mjcf.3433/#post-5423
* Add tag to urdf file
  ```xml
  <mujoco>
        <compiler 
        meshdir="../meshes_mujoco/" 
        balanceinertia="true" 
        discardvisual="false" />
  </mujoco>
  ```
  Note that this tag is under \<robot name="sawyer" xmlns:xacro="http://www.ros.org/wiki/xacro"\>

* Mujoco does not support dae. Therefore, we need to convert dae file to stl file. Meshlab can be used.

* Do other necessary changes to the urdf file. For example, path to mesh files, change .dae to .stl etc.

* Use compile in mujoco/bin to convert urdf to mjdf
  ```
  $./compile /path/to/model.urdf /path/to/model.xml

  ```
Notes:
* After converting, the mjdf file will have some geoms with type box, cylinder etc. They are for collision detection. The mesh geom cannot be used for collision detection. In simulation, just not render group 0 will make them not visulizable.

## MUJOCO
* The quantities in mjData that start with "x" are expressed in global coordinates. Refer to **Coordinate frames and transformations** section in http://www.mujoco.org/book/programming.html#siCoordinate for detail.
## PYTHON
* In fact, In fact, Python formally acknowledges that the names defined as for loop targets (a more formally rigorous name for "index variables") leak into the enclosing function scope. See https://eli.thegreenplace.net/2015/the-scope-of-index-variables-in-pythons-for-loops/ for more information. 


## Installing Vim-key bindings for jupyter notebook
* ```bash
  pip install jupyter_contrib_nbextensions

  ```
* ```bash
  jupyter nbextensions_configurator enable --user
  ```
* ```bash
  # You may need the following to create the directoy
  mkdir -p $(jupyter --data-dir)/nbextensions
  # Now clone the repository
  cd $(jupyter --data-dir)/nbextensions
  git clone https://github.com/lambdalisue/jupyter-vim-binding vim_binding
  chmod -R go-w vim_binding
  ```
* Launch a Jupyter notebook session. Then, in a browser, go to <root>/nbextensions/; for example, if the notebook is hosted under localhost:8888, go to http://localhost:8888/nbextensions/. Activate VIM binding from the list of extensions. Check documentation for more details.

## Machine Learning
* During using supervised learning to train a mlp, I encounter a strange problem. In the beginning of each epoch, the loss will jump, and then slowly decrease. It turns out to be that numpy's shuffle's randomness is not enough. Using pytorch's dataloader's shuffle solves this problem.

## Tmux
* Add the following to ~/.tmux.conf to make tmux more user friendly
```
# remap prefix from 'C-b' to 'C-a'
unbind C-b
set-option -g prefix C-a
bind-key C-a send-prefix
# set mode to vi mode
set-window-option -g mode-keys vi
```
For more information, refer to this blog https://www.hamvocke.com/blog/a-guide-to-customizing-your-tmux-conf/

