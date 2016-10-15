FROM kgopalak/ubuntu-dev-base:latest

MAINTAINER krishna.gopalakrishnan@gmail.com

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

RUN wget https://az764295.vo.msecnd.net/stable/9e4e44c19e393803e2b05fe2323cf4ed7e36880e/code_1.6.1-1476373175_amd64.deb
RUN dpkg -i code_1.6.1-1476373175_amd64.deb

RUN rm -rf /sublime-text_build-3126_amd64.deb 
RUN rm -rf /Anaconda2-4.2.0-Linux-x86_64.sh
RUN rm -rf /vendor_build
RUN rm -rf /root/vendor_build

# Install Catkin from http://catkin-tools.readthedocs.io/en/latest/installing.html
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu `lsb_release -sc` main" > /etc/apt/sources.list.d/ros-latest.list'
RUN wget http://packages.ros.org/ros.key -O - | sudo apt-key add -
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 					apt-get -y install \
 					python-catkin-tools \
 					&& rm -rf /var/lib/apt/lists/*

WORKDIR /root
RUN bash