FROM kgopalak/perception-dev:latest

MAINTAINER krishna.gopalakrishnan@gmail.com

RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update &&\
 					apt-get -y install \
 					clang \
                    libicu-dev \
 					&& rm -rf /var/lib/apt/lists/* 

# Install latest SWIFT
ENV SWIFT_VERSION=3.0.1-preview-3
ENV SWIFT_PLATFORM=ubuntu16.04
ENV swift=swift-${SWIFT_VERSION}-${SWIFT_PLATFORM}
RUN wget -q -O - https://swift.org/keys/all-keys.asc | gpg --import -
RUN gpg --keyserver hkp://pool.sks-keyservers.net --refresh-keys Swift
RUN wget https://swift.org/builds/swift-3.0.1-preview-3/ubuntu1604/swift-3.0.1-PREVIEW-3/swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz
RUN mkdir -p /opt/swift
RUN mv swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz /opt/swift 
RUN tar xzf /opt/swift/swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz -C /opt/swift 
RUN rm /opt/swift/swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz
RUN export PATH=/opt/swift/swift-3.0.1-PREVIEW-3-ubuntu16.04/usr/bin:"${PATH}"
RUN wget https://swift.org/builds/swift-3.0.1-preview-3/ubuntu1604/swift-3.0.1-PREVIEW-3/swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz.sig
#RUN gpg --verify swift-3.0.1-PREVIEW-3-ubuntu16.04.tar.gz.sig

WORKDIR /root
RUN bash