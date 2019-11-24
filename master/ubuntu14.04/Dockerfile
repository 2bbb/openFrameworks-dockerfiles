FROM ubuntu:14.04
LABEL maintainer="2bit <i@2bit.jp>"

ARG REPO_USER=openframeworks

RUN echo "now building openFrameworks master on Ubuntu 14.04 with ${REPO_USER}'s master branch"
RUN apt-get update && apt-get -y upgrade
RUN apt-get install -y software-properties-common
RUN add-apt-repository ppa:ubuntu-toolchain-r/test
RUN apt-get update && apt-get -y upgrade
RUN apt-get update && apt install -y \
    curl \
    wget \
    lsb-release \
    git \
    vim

RUN cd root && \
    git clone --recursive https://github.com/${REPO_USER}/openFrameworks.git && \
    openFrameworks/scripts/linux/ubuntu/install_dependencies.sh -y

RUN echo "export OF_ROOT=/root/openFrameworks" >> /root/.bashrc
RUN echo "alias pg=\"projectGenerator -o\\\"/root/openFrameworks\\\"\"" >> /root/.bashrc

RUN cd root && \
    openFrameworks/scripts/linux/download_libs.sh -s

RUN export GSTREAMER_VERSION=0.10 && \
    apt-cache show libgstreamer1.0-dev && export GSTREAMER_VERSION=1.0 ; \
    apt-get install libmpg123-dev gstreamer${GSTREAMER_VERSION}-plugins-ugly -y

RUN echo "export LC_ALL=C.UTF-8" >> /root/.bashrc

RUN cd root && \
    openFrameworks/scripts/linux/compileOF.sh -j$(($(nproc) - 1)) 

RUN make Release -C /root/openFrameworks/apps/projectGenerator/commandLine && \
    cp /root/openFrameworks/apps/projectGenerator/commandLine/bin/projectGenerator /usr/bin/projectGenerator && \
    projectGenerator -o"/root/openFrameworks" /root/openFrameworks/apps/myApps/testProject

RUN echo -e "#!/bin/bash\n\
\n\
cd /root/openFrameworks && \n\
LC_ALL=C.UTF-8 && \n\
./scripts/ci/linux64/run_tests.sh" > /root/test.sh && \
    chmod +x /root/test.sh


CMD echo "now oF master env is ready on Ubuntu 14.04"
