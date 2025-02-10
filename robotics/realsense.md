# Installation
## `librealsense2`
`librealsense2` is required for using `realsense-ros`.

Reference: [official doc](https://github.com/IntelRealSense/librealsense/blob/master/doc/distribution_linux.md)

Installation steps:
1. Register the server's public key:
    ```
    sudo mkdir -p /etc/apt/keyrings
    curl -sSf https://librealsense.intel.com/Debian/librealsense.pgp | sudo tee /etc/apt/keyrings/librealsense.pgp > /dev/null
    ``` 
2. Install apt HTTPS support
    ```
    sudo apt-get install apt-transport-https
    ```
3. Add the server to the list of repositories
    ```
    echo "deb [signed-by=/etc/apt/keyrings/librealsense.pgp] https://librealsense.intel.com/Debian/apt-repo $(lsb_release -cs) main" | \
    sudo tee /etc/apt/sources.list.d/librealsense.list
    sudo apt-get update
    ``` 
    > Original command in the official doc has ``` `lsb_release -cs` ``` instead of `$(lsb_release -cs)`. However, ``` `command` ``` is not supported in fish. `$(command)` should be used instead.
4. Install related packages:
    ```
    sudo apt-get install librealsense2-dkms
    sudo apt-get install librealsense2-utils
    sudo apt-get install librealsense2-dev
    sudo apt-get install librealsense2-dbg
    ```
    - `librealsense2-dev` is required for compilation, which is required in working with ROS.


# ROS
- Start the realsense camera with
    ``` 
    roslaunch realsense2_camera rs_camera.launch align_depth:=true
    ```
  The realsense related topic will be published.
- `Realsense-ROS` provides basic functionality to visualize the point cloud. Check it with:
    ```
    roslaunch realsense2_camera demo_pointcloud.launch
    ```