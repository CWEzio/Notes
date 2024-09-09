

# Usages
## Convert urdf to mjcf
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
> This is an old note. I believe there now is a better method or tool available. I will update this note in the future after encountering this problem again.
