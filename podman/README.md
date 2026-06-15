# Create software image
```
podman pull docker.io/osrf/ros:jazzy-desktop-full
podman run -it --rm --name kiss_icp docker.io/osrf/ros:jazzy-desktop-full
```

```
   apt update && apt -y upgrade
   mkdir -p /ros2_ws/src
   cd ros2_ws/src/
   git clone https://github.com/ecervera/kiss-icp
   cd .. && colcon build
```

```
podman commit kiss_icp ghcr.io/ecervera/kiss_icp:latest
podman push ghcr.io/ecervera/kiss_icp:latest
```

# Run container

```
xhost +local:$USER
podman run -it --rm --name kiss_icp \
    --env="DISPLAY=$DISPLAY" \
    --env="QT_X11_NO_MITSHM=1" \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
    --device /dev/dri \
    ghcr.io/ecervera/kiss_icp:latest
```
