#Base docker image
FROM ubuntu:14.04
RUN apt-get -y update
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:george-edison55/cmake-3.x -y
RUN apt-get -y update
RUN apt-get -y install python-scipy gawk git python-pip  
RUN echo "export PS1=\\\\\\\\w\\$" >> /etc/bash.bashrc
RUN apt-get -y install xterm
RUN pip install --upgrade pip
RUN apt-get -y install cmake 
RUN apt-get -y install build-essential
RUN apt-get -y install gcc
#RUN apt-get -y install clang
RUN apt-get -y install unzip
RUN apt-get -y install vim 
RUN apt-get -y install wget curl
#python packages
RUN apt-get -y install python-zmq

#window conrtol
RUN apt-get -y install wmctrl xdotool

#### tmux new version
RUN apt-get -y install libprotoc-dev 
RUN apt-get -y install protobuf-compiler
RUN apt-get -y install pkg-config

RUN apt-get -y install libevent1-dev libncurses5-dev
ENV LC_CTYPE=C.UTF-8
RUN cd /tmp && git clone https://github.com/tmux/tmux.git && cd tmux && git checkout tags/2.3
RUN apt-get -y install automake
RUN cd /tmp/tmux && sh ./autogen.sh && ./configure && make install

#to enable copy paste in tmux mouse on https://coderwall.com/p/4b0d0a/how-to-copy-and-paste-with-tmux-on-ubuntu
RUN apt-get -y install xclip

#python3
RUN apt-get -y install curl
RUN curl -o /miniconda.sh https://repo.continuum.io/miniconda/Miniconda3-4.2.12-Linux-x86_64.sh
RUN /bin/bash /miniconda.sh -b -p /miniconda
RUN PATH=/miniconda/bin conda install -y pyzmq
RUN PATH=/miniconda/bin conda install -y numpy


######## ros indigo ######
RUN echo "deb http://packages.ros.org/ros/ubuntu trusty main" > /etc/apt/sources.list.d/ros-latest.list
RUN apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net --recv-key 0xB01FA116
RUN apt-get -y update
RUN apt-get -y install ros-indigo-desktop
RUN apt-get -y install python-wstool python-rosinstall-generator python-catkin-tools
RUN bash -c "curl -ssL http://get.gazebosim.org | sh"
RUN apt-get -y install ros-indigo-gazebo7-ros-pkgs
RUN apt-get -y install ros-indigo-roslaunch
RUN apt-get -y install gazebo7
RUN apt-get -y install libgazebo7-dev
RUN apt-get -y install ros-indigo-control-toolbox
#RUN apt-get -y install openssl libssl-dev
RUN apt-get -y install ros-indigo-mavros ros-indigo-mavros-extras
RUN apt-get -y install ros-indigo-image-view2

#nvidia
ARG GDRIVER
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y $GDRIVER
RUN update-locale LANG=C LANGUAGE=C LC_ALL=C LC_MESSAGES=POSIX
ENV QT_X11_NO_MITSHM 1

#additional ros libs
RUN apt-get -y install ros-indigo-image-pipeline
RUN apt-get -y install libglew-dev #nedded by Pangolin viewer
# git checkout tags/v0.5
# .... build
RUN cd /tmp && git clone https://github.com/stevenlovegrove/Pangolin.git
RUN cd /tmp/Pangolin && git checkout tags/v0.5 && mkdir build && cd build && cmake .. && make
###### ORB_SLAM2
RUN cd /tmp && git clone https://github.com/raulmur/ORB_SLAM2.git 
RUN cd /tmp/ORB_SLAM2 && chmod +x build.sh 
RUN cd /tmp/ORB_SLAM2 && chmod +x build_ros.sh 
RUN cd /tmp/ORB_SLAM2 && ./build.sh
#RUN cd /tmp/ORB_SLAM2 && /bin/bash -c "source /opt/ros/indigo/setup.bash && export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:/tmp/ORB_SLAM2/Examples/ROS && rosdep update && ./build_ros.sh"
#RUN cd /tmp/ORB_SLAM2 && /bin/bash -c "export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:/tmp/ORB_SLAM2/Examples/ROS && ./build_ros.sh"

RUN chmod 777 /tmp/ORB_SLAM2/build


#user setings
ARG UID
RUN useradd -u $UID docker
RUN echo "docker:docker" | chpasswd
RUN echo "docker ALL=(ALL:ALL) NOPASSWD:ALL" >> /etc/sudoers 



