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