FROM ros:humble

RUN apt-get update && apt-get install -y curl ros-humble-ros2-control ros-humble-image-transport

RUN curl https://raw.githubusercontent.com/choreonoid/choreonoid/master/misc/script/install-requisites-ubuntu-22.04.sh -o install-requisites-ubuntu-22.04.sh

RUN /bin/sh install-requisites-ubuntu-22.04.sh

RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc

RUN touch /root/.bash_history

RUN echo "source install/setup.bash" >> /root/.bash_history

RUN echo "colcon build --symlink-install --cmake-args -DCMAKE_BUILD_TYPE=Release" >> /root/.bash_history

RUN echo "ros2 launch choreonoid_ros choreonoid.launch.xml" >> /root/.bash_history

WORKDIR /root/ros2_ws