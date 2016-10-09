FROM kgopalak/ubuntu-dev-base:1.0

MAINTAINER krishna.gopalakrishnan@gmail.com

# Prerequisites for Opencv
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 		apt-get -y install \
		libgtk2.0-dev \
		 libavcodec-dev \
		 libavformat-dev \
		 libswscale-dev \
		 libxine2-dev \
		 libtbb2 \
		 libtbb-dev \
		 libjpeg-dev \
		 libpng-dev \
		 libtiff-dev \
		 libtiff5-dev \
		 libjasper-dev \
		 libdc1394-22-dev \
		 libprotobuf-dev \
		 doxygen \
		 graphviz \
		 libeigen3-dev \
		 doxygen \
		 ffmpeg \
		 libgtk-3-dev \
		 libgstreamer1.0-dev \
		 libgstreamer-plugins-base1.0-dev \
		 libv4l-dev \
		 qtbase5-dev \
		 libfaac-dev \
		 libmp3lame-dev \
		 libopencore-amrnb-dev \
		 libopencore-amrwb-dev \
		 libtheora-dev \
		 libvorbis-dev \
		 libxvidcore-dev \
		 x264 \
		 v4l-utils \
		 libbz2-dev \
		 libhdf5-dev \
		 && rm -rf /var/lib/apt/lists/*

# Get and build OpenCV 3.1
WORKDIR /root/vendor_build/opencv/3.1.0
RUN wget https://github.com/Itseez/opencv/archive/3.1.0.zip 
RUN wget https://github.com/Itseez/opencv_contrib/archive/3.1.0.zip -O contrib-3.1.0.zip
RUN unzip 3.1.0.zip
RUN unzip contrib-3.1.0.zip -d contrib-3.1.0
## This is needed to fix a build error. forgot to inclue HDF5 package
RUN sed -i '1s/^/find_package(HDF5)\n include_directories(${HDF5_INCLUDE_DIRS})/' /root/vendor_build/opencv/3.1.0/opencv-3.1.0/modules/python/common.cmake
WORKDIR build
RUN cmake -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=/usr/local -DWITH_TBB=ON -DBUILD_NEW_PYTHON_SUPPORT=ON -DWITH_V4L=ON -DWITH_OPENGL=ON -DWITH_CUDA=OFF -DWITH_QT=ON -DWITH_OPENMP=ON -DOPENCV_EXTRA_MODULES_PATH=/root/vendor_build/opencv/3.1.0/contrib-3.1.0/opencv_contrib-3.1.0/modules/ /root/vendor_build/opencv/3.1.0/opencv-3.1.0/
RUN make -j $(nproc) && make install
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/opencv.conf
RUN ldconfig

# BOOST C++ 
ARG boost_version=1.62.0
ARG boost_dir="boost_1_62_0"
ENV boost_version ${boost_version}
WORKDIR /vendor_build/boost/${boost_version}
RUN wget http://downloads.sourceforge.net/project/boost/boost/${boost_version}/${boost_dir}.tar.gz 
RUN tar -xf ${boost_dir}.tar.gz
WORKDIR ${boost_dir}
RUN /vendor_build/boost/${boost_version}/${boost_dir}/bootstrap.sh \
    && ./b2 --prefix=/usr/local -j 8 link=shared threading=multi address-model=64 variant=release runtime-link=shared install \
    && ldconfig

RUN rm -rf sublime-text_build-3126_amd64.deb 
RUN rm -rf Anaconda2-4.2.0-Linux-x86_64.sh
RUN rm -rf /vendor_build
RUN rm -rf /root/vendor_build