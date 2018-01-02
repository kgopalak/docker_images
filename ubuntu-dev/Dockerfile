# This is ubuntu-dev-base image

# This file describes how to build into a runnable linux container with all dependencies installed
# Build: wget https://raw.github.com/dotcloud/docker/v0.1.6/contrib/docker-build/docker-build && python docker-build $USER/kg-dev-base < Dockerfile
# Test: docker run $USER/kg-dev-base node -e "console.log(require('opencv').version)"

FROM ubuntu:16.04

MAINTAINER krishna.gopalakrishnan@gmail.com

# this is needed as libfaac-dev comes from multiverse, according to:
# http://superuser.com/questions/467774/how-to-install-libfaac-dev
RUN echo "deb http://us.archive.ubuntu.com/ubuntu/ precise multiverse\n\
deb-src http://us.archive.ubuntu.com/ubuntu/ precise multiverse\n\
deb http://us.archive.ubuntu.com/ubuntu/ precise-updates multiverse\n\
deb-src http://us.archive.ubuntu.com/ubuntu/ precise-updates multiverse\n"\
>> /etc/apt/sources.list

RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update && \
	apt-get -y install \
   	software-properties-common \
    && rm -rf /var/lib/apt/lists/*

RUN add-apt-repository ppa:george-edison55/cmake-3.x
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 apt-get -y install \
 build-essential \
 curl \
 cmake \
 git \
 gdb \
 libgtk2.0-dev \
 pkg-config \
 python-dev \
 libdc1394-22 \
 libdc1394-22-dev \
 libjpeg-dev \
 libpng12-dev \
 libpng-dev \
 libtiff5-dev \
 libtiff5-dev \
 libjasper-dev \
 libtbb2 \
 libtbb-dev \
 libavcodec-dev \
 libavformat-dev \
 libswscale-dev \
 libxine2-dev \
 libgstreamer0.10-dev \
 libgstreamer-plugins-base0.10-dev \
 libv4l-dev \
 libtbb-dev \
 libqt4-dev \
 libfaac-dev \
 libmp3lame-dev \
 libdc1394-22-dev \
 libgtk-3-dev \
 libgstreamer1.0-dev \
 libgstreamer-plugins-base1.0-dev \
 libv4l-dev \
 libopencore-amrnb-dev \
 libopencore-amrwb-dev \
 libtheora-dev \
 libvorbis-dev \
 libxvidcore-dev \
 x264 \
 v4l-utils \
 terminator \
 wget \
 unzip \
 libbz2-dev \
 libprotobuf-dev \
 doxygen \
 graphviz \
 libeigen3-dev \
 doxygen \
 ffmpeg \
 qtbase5-dev \
 libhdf5-dev \
 terminator \
	&& rm -rf /var/lib/apt/lists/*

#JDK
RUN add-apt-repository ppa:openjdk-r/ppa
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 apt-get -y install \
	openjdk-8-jdk \
&& rm -rf /var/lib/apt/lists/*
RUN update-alternatives --config java
RUN update-alternatives --config javac

RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 apt-get -y install \
 	emacs \
 	gnome-terminal \
 	&& rm -rf /var/lib/apt/lists/*

# Sublime Text 3
RUN wget https://download.sublimetext.com/sublime-text_build-3126_amd64.deb
RUN dpkg -i  sublime-text_build-3126_amd64.deb
#RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
# 	apt-get -qqy install \
# 	sublime \
# 	&& rm -rf /var/lib/apt/lists/*
RUN rm sublime-text_build-3126_amd64.deb

### continuumio/anaconda
RUN wget https://repo.continuum.io/archive/Anaconda2-4.2.0-Linux-x86_64.sh
RUN chmod a+rwx Anaconda2-4.2.0-Linux-x86_64.sh
RUN ./Anaconda2-4.2.0-Linux-x86_64.sh -b -p /opt/conda
RUN echo 'export PATH=/opt/conda/bin:$PATH' > /etc/profile.d/conda.sh
ENV PATH /opt/conda/bin:$PATH
RUN rm Anaconda2-4.2.0-Linux-x86_64.sh
### continuumio/anaconda

# Eclipse
WORKDIR /opt/eclipse/neon
RUN wget http://ftp.osuosl.org/pub/eclipse/technology/epp/downloads/release/neon/R/eclipse-cpp-neon-R-linux-gtk-x86_64.tar.gz
RUN tar -xf eclipse-cpp-neon-R-linux-gtk-x86_64.tar.gz

# Get CUDA SDK
RUN wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1604/x86_64/cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
#RUN wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/cuda-repo-ubuntu1404_7.5-18_amd64.deb
#RUN dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
RUN dpkg -i cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
RUN rm cuda-repo-ubuntu1604_8.0.44-1_amd64.deb
#RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
# 	apt-get -y install cuda && rm -rf /var/lib/apt/lists/*

# Zsh and oh-my-zsh
#RUN apt-get -qq update \
# 	&& DEBIAN_FRONTEND=noninteractive apt-get -qqy install \
# 	zsh \
# 	&& rm -rf /var/lib/apt/lists/*
#RUN wget https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh
#RUN chmod a+rwx install.sh
#RUN ./install.sh -O - | zsh
#RUN chsh -s `which zsh`

#CMD zsh
