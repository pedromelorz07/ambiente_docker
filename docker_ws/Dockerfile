FROM osrf/ros:humble-desktop-full

ENV DEBIAN_FRONTEND=noninteractive
ENV ROS_DISTRO=humble

# Adicionamos os pacotes do turtlebot3 e simulações aqui
RUN apt-get update && apt-get install -y \
    nano \
    vim \
    git \
    curl \
    wget \
    build-essential \
    python3-pip \
    ros-humble-turtlesim \
    ros-humble-rviz2 \
    ros-humble-gazebo-ros \
    ros-humble-turtlebot3 \
    ros-humble-turtlebot3-msgs \
    ros-humble-turtlebot3-simulations \
    gazebo \
    xauth && \
    rm -rf /var/lib/apt/lists/*

RUN echo "source /opt/ros/humble/setup.bash" >> /root/.bashrc

# Configuração extra necessária para o TurtleBot3
# Define o modelo padrão (pode ser burger, waffle ou waffle_pi)
ENV TURTLEBOT3_MODEL=burger
RUN echo "export TURTLEBOT3_MODEL=burger" >> /root/.bashrc

WORKDIR /root/workspace

CMD ["/bin/bash"]