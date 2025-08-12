# direct_visual_lidar_calibration

This package provides a toolbox for LiDAR-camera calibration that is: 


**Documentation: [https://koide3.github.io/direct_visual_lidar_calibration/](https://koide3.github.io/direct_visual_lidar_calibration/)**  

## How to use with Docker
- **Permissions**
    ```bash
    xhost +local:root
    ```
- **Preproccesing**
    ```bash
    docker run --rm \
    --gpus all \
    --net=host \
    --ipc=host \
    --privileged \
    -e DISPLAY=$DISPLAY \
    -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
    -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
    -e PULSE_SERVER=$PULSE_SERVER \
    -e QT_X11_NO_MITSHM=1 \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
    -e NVIDIA_VISIBLE_DEVICES=all \
    -e ROS_DOMAIN_ID=69 \
    -v $HOME/.Xauthority:/root/.Xauthority \
    -v /home/tarun/insta360:/tmp/input_bags \
    -v /home/tarun/insta360/result:/tmp/preprocessed \
    koide3/direct_visual_lidar_calibration:humble \
    ros2 run direct_visual_lidar_calibration preprocess \
    --image_topic /camera1/camera1/color/image_raw \
    --camera_info_topic /camera1/camera1/color/camera_info \
    --points_topic /livox/lidar \
    -v /tmp/input_bags /tmp/preprocessed
    ```
- **Initial guess(Manual)**
    ```bash
    docker run --rm \
    --gpus all \
    --net=host \
    --ipc=host \
    --privileged \
    -e DISPLAY=$DISPLAY \
    -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
    -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
    -e PULSE_SERVER=$PULSE_SERVER \
    -e QT_X11_NO_MITSHM=1 \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
    -e NVIDIA_VISIBLE_DEVICES=all \
    -e ROS_DOMAIN_ID=69 \
    -v $HOME/.Xauthority:/root/.Xauthority \
    -v /home/tarun/livox/result:/tmp/preprocessed \
    koide3/direct_visual_lidar_calibration:humble \
    ros2 run direct_visual_lidar_calibration initial_guess_manual \
    /tmp/preprocessed
    ```

- **Fine registration**
    ```bash
    docker run --rm \
    --gpus all \
    --net=host \
    --ipc=host \
    --privileged \
    -e DISPLAY=$DISPLAY \
    -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
    -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
    -e PULSE_SERVER=$PULSE_SERVER \
    -e QT_X11_NO_MITSHM=1 \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
    -e NVIDIA_VISIBLE_DEVICES=all \
    -e ROS_DOMAIN_ID=69 \
    -v $HOME/.Xauthority:/root/.Xauthority \
    -v /home/tarun/livox/result:/tmp/preprocessed \
    koide3/direct_visual_lidar_calibration:humble \
    ros2 run direct_visual_lidar_calibration calibrate \
    /tmp/preprocessed
    ```
- **View output (optional)**
    ```bash
    docker run --rm \
    --gpus all \
    --net=host \
    --ipc=host \
    --privileged \
    -e DISPLAY=$DISPLAY \
    -e WAYLAND_DISPLAY=$WAYLAND_DISPLAY \
    -e XDG_RUNTIME_DIR=$XDG_RUNTIME_DIR \
    -e PULSE_SERVER=$PULSE_SERVER \
    -e QT_X11_NO_MITSHM=1 \
    -e NVIDIA_DRIVER_CAPABILITIES=all \
    -e NVIDIA_VISIBLE_DEVICES=all \
    -e ROS_DOMAIN_ID=69 \
    -v $HOME/.Xauthority:/root/.Xauthority \
    -v /home/tarun/livox/result:/tmp/preprocessed \
    koide3/direct_visual_lidar_calibration:humble \
    ros2 run direct_visual_lidar_calibration viewer \
    /tmp/preprocessed
    ```

## Result

- Once the calibration is completed, open `calib.json` in the data directory with a text editor and find the calibration result `T_lidar_camera: [x, y, z, qx, qy, qz, qw]` that transforms a 3D point in the camera frame into the LiDAR frame (i.e., `p_lidar = T_lidar_camera * p_camera`).





