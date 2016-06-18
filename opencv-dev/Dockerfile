# This file describes how to build node-opencv into a runnable linux container with all dependencies installed
# To build:
# 1) Install docker (http://docker.io)
# 2) Build: wget https://raw.github.com/dotcloud/docker/v0.1.6/contrib/docker-build/docker-build && python docker-build $USER/node-opencv < Dockerfile
# 3) Test: docker run $USER/node-opencv node -e "console.log(require('opencv').version)"
#
# VERSION		0.3
# DOCKER-VERSION	8.1.2

# update to 14.04
FROM ubuntu:14.04

# listing myself as maintainer of _this_ Dockerfile, though I am not the author of the install script
MAINTAINER krishna.gopalakrishnan@gmail.com

# this is needed as libfaac-dev comes from multiverse, according to:
# http://superuser.com/questions/467774/how-to-install-libfaac-dev
RUN echo "deb http://us.archive.ubuntu.com/ubuntu/ precise multiverse\n\
deb-src http://us.archive.ubuntu.com/ubuntu/ precise multiverse\n\
deb http://us.archive.ubuntu.com/ubuntu/ precise-updates multiverse\n\
deb-src http://us.archive.ubuntu.com/ubuntu/ precise-updates multiverse\n"\
>> /etc/apt/sources.list
RUN apt-get update -qq
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
    software-properties-common
RUN add-apt-repository ppa:george-edison55/cmake-3.x
RUN apt-get update -qq

RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
	curl \
	cmake \
	wget \
	unzip \
	build-essential \
	git \
	libgtk2.0-dev \
	pkg-config \
	python-dev \
	python-numpy \
	libdc1394-22 \
	libdc1394-22-dev \
	libjpeg-dev \
	libpng12-dev \
	libtiff4-dev \
	libjasper-dev \
	libtbb2 \ 
	libavcodec-dev \
	libavformat-dev \
	libswscale-dev \
	libxine-dev \
	libgstreamer0.10-dev \
	libgstreamer-plugins-base0.10-dev \
	libv4l-dev \
	libtbb-dev \
	libqt4-dev \
	libfaac-dev \
	libmp3lame-dev \
	libdc1394-22-dev \
	libopencore-amrnb-dev \
	libopencore-amrwb-dev \
	libtheora-dev \
	libvorbis-dev \
	libxvidcore-dev \
	x264 \
	v4l-utils

# Get CUDA SDK
RUN wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_7.5-18_amd64.deb
RUN dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
	cuda

# Get and build OpenCV 3.1
WORKDIR vendor_build/opencv/3.1.0
RUN wget https://github.com/Itseez/opencv/archive/3.1.0.zip 
RUN wget https://github.com/Itseez/opencv_contrib/archive/3.1.0.zip -O contrib-3.1.0.zip
RUN unzip 3.1.0.zip
RUN unzip contrib-3.1.0.zip -d contrib-3.1.0
WORKDIR build
RUN pwd
RUN ls -l
RUN cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON -DWITH_OPENGL=ON -DWITH_CUDA=ON -DOPENCV_EXTRA_MODULES_PATH=/vendor_build/opencv/3.1.0/contrib-3.1.0/opencv_contrib-3.1.0/modules/ /vendor_build/opencv/3.1.0/opencv-3.1.0/
RUN make -j $(nproc) && make install
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.vendor_build/opencv/3.1.0conf
RUN ldconfig

# Eclipse
RUN add-apt-repository ppa:openjdk-r/ppa
RUN apt-get update
RUN DEBIAN_FRONTEND=noninteractive apt-get install -y \
	openjdk-8-jdk
RUN update-alternatives --config java
RUN update-alternatives --config javac
RUN wget http://eclipse.mirror.rafal.ca/technology/epp/downloads/release/neon/RC3/eclipse-cpp-neon-RC3-linux-gtk-x86_64.tar.gz
RUN tar -xf eclipse-cpp-neon-RC3-linux-gtk-x86_64.tar.gz -O /opt/eclipse-neon/
