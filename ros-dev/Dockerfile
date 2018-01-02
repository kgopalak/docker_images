FROM osrf/ros:kinetic-desktop-full
MAINTAINER Krishna Gopalakrishnan <krishna.gopalakrishnan@gmail.com>

# Set the time zone to EST
RUN echo "America/New_York" > /etc/timezone
RUN dpkg-reconfigure -f noninteractive tzdata

# Setup dowloads
WORKDIR /root/Downloads

RUN echo 'Installing EMACS, GEEQIE, GITK, MELD'
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
    cmake \
    emacs \
    gedit \
    geeqie \
    gnome-terminal \
    gitk \
    default-jdk \
    libc6-dbg  \
    meld \
    meshlab \
    nano \
    terminator \
    valgrind \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
      apt-utils \
      wget \
      && rm -rf /var/lib/apt/lists/*

RUN echo 'Installing CUDA 9.0'
RUN wget -q https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda_9.0.176_384.81_linux-run
RUN chmod a+x cuda_9.0.176_384.81_linux-run
RUN ./cuda_9.0.176_384.81_linux-run --silent --toolkit
RUN rm cuda_9.0.176_384.81_linux-run
RUN echo "export PATH=/usr/local/cuda-9.0/bin:\$PATH" >> ~/.bashrc
RUN echo "export LD_LIBRARY_PATH=/usr/local/cuda-9.0/lib64:\$LD_LIBRARY_PATH" >> ~/.bashrc

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
      libeigen3-dev \
      libmatheval-dev \
      libjpeg-dev \
      libpng-dev \
      libtinyxml-dev \
      && rm -rf /var/lib/apt/lists/*

RUN echo 'Anaconda'
RUN wget -q https://repo.continuum.io/archive/Anaconda2-5.0.1-Linux-x86_64.sh && \
    chmod +x Anaconda2-5.0.1-Linux-x86_64.sh && \
    ./Anaconda2-5.0.1-Linux-x86_64.sh -b -p /opt/anaconda2-5/

RUN echo 'ROS Python tools'
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
      python-rosinstall \
      python-rosinstall-generator \
      python-wstool \
      build-essential

RUN apt-get install libgtest-dev && \
    cd /usr/src/gtest && \
    mkdir build && \
    cd build && \
    cmake .. && \
    make && \
    mv libgtest* /usr/lib/

RUN echo 'Misc tools'
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
     mesa-utils \
     module-init-tools \
     && rm -rf /var/lib/apt/lists/*

ENV NVIDIA_FILE=NVIDIA-Linux-x86_64-384.90.run
RUN wget -q http://us.download.nvidia.com/XFree86/Linux-x86_64/384.90/${NVIDIA_FILE}
RUN chmod a+x ${NVIDIA_FILE}
RUN ./${NVIDIA_FILE} -a -N --ui=none --no-kernel-module
RUN rm ${NVIDIA_FILE}

RUN echo 'Installing Pangolin'
RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
     libglew-dev && rm -rf /var/lib/apt/lists/* && \
     git clone https://github.com/stevenlovegrove/Pangolin.git && \
     cd Pangolin && \
     mkdir build && \
     cd build && \
     cmake .. && \
     cmake --build .

RUN echo 'Installing Intel Realsense 2.9.0'
RUN echo 'deb http://realsense-hw-public.s3.amazonaws.com/Debian/apt-repo xenial main' | tee /etc/apt/sources.list.d/realsense-public.list \
    && apt-key adv --keyserver keys.gnupg.net --recv-key 7DCC6F0F \
    && apt-get update && DEBIAN_FRONTEND=noninteractive apt-get -y install \
        realsense-uvcvideo \
        librealsense2-utils \
        librealsense2-dev \
        librealsense2-dbg \
    && rm -rf /var/lib/apt/lists/*

RUN echo "export QT_X11_NO_MITSHM=1" >> /root/.bashrc
WORKDIR /root

# Sourcing this before .bashrc runs breaks ROS completions
RUN echo "\nsource /opt/ros/kinetic/setup.bash" >> /root/.bashrc
CMD ["bash"]
