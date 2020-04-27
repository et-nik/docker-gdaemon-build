FROM ubuntu:xenial

RUN apt-get -y update && apt-get -y install \
        curl \
        wget \
        gnupg2 \
        software-properties-common \
    --no-install-recommends && rm -r /var/lib/apt/lists/* \
    && update-alternatives --install /usr/bin/gcc gcc /usr/bin/gcc-9 60 --slave /usr/bin/g++ g++ /usr/bin/g++-9

RUN add-apt-repository ppa:ubuntu-toolchain-r/test -y \
    && curl -fsSL http://packages.gameap.ru/gameap-rep.gpg.key | apt-key add - \
    && echo "deb http://packages.gameap.ru/ubuntu xenial main" > /etc/apt/sources.list.d/gameap.list

RUN apt-get -y update && apt-get -y install \
        gcc-9 \
        g++-9 \
        cmake \
        pkg-config \
        libbinn-dev \
        librestclientcpp-dev \
        libjsoncpp-dev \
        libssl-dev \
        libcurl4-openssl-dev \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN wget http://packages.gameap.ru/others/boost_1_70_0.tar.gz \
    && tar -xvf boost_1_70_0.tar.gz \
    && cd boost_1_70_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,serialization,system,thread,test \
    && ./b2 install --link=static -j $(grep -c ^processor /proc/cpuinfo) \
    && cd .. && rm -r boost_1_70_0 && rm boost_1_70_0.tar.gz
