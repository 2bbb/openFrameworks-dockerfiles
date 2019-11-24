FROM ubuntu:16.04
LABEL maintainer="2bit <i@2bit.jp>"

RUN echo "now building openFrameworks 0.10.1 on Ubuntu 16.04"
RUN apt update && apt install -y \
    curl \
    lsb-release \
    vim

RUN cd root && \
    curl -O https://openframeworks.cc/versions/v0.10.1/of_v0.10.1_linux64gcc6_release.tar.gz 2> /dev/null && \
    tar -xf of_v0.10.1_linux64gcc6_release.tar.gz && \
    rm of_v0.10.1_linux64gcc6_release.tar.gz && \
    mv of_v0.10.1_linux64gcc6_release of_v0.10.1 && \
    of_v0.10.1/scripts/linux/ubuntu/install_dependencies.sh -y
RUN echo "export OF_ROOT=/root/of_v0.10.1" >> /root/.bashrc
RUN echo "alias pg=\"projectGenerator -o\\\"/root/of_v0.10.1\\\"\"" >> /root/.bashrc

RUN export GSTREAMER_VERSION=0.10 && \
    apt-cache show libgstreamer1.0-dev && export GSTREAMER_VERSION=1.0 ; \
    apt-get install libmpg123-dev gstreamer${GSTREAMER_VERSION}-plugins-ugly -y
RUN cd root && \
    of_v0.10.1/scripts/linux/compileOF.sh -j$(($(nproc) - 1)) 
RUN export LC_ALL=C && \
    export OF_ROOT=/root/of_v0.10.1 && \
    make Release -C ${OF_ROOT}/apps/projectGenerator/commandLine && \
    cp ${OF_ROOT}/apps/projectGenerator/commandLine/bin/projectGenerator /usr/bin/projectGenerator && \
    projectGenerator -o"${OF_ROOT}" ${OF_ROOT}/apps/myApps/testProject

CMD echo "now oF 0.10.1 env is ready on Ubuntu 16.04"
