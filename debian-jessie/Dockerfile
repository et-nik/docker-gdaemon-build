FROM buildpack-deps:jessie

RUN apt-get -y update && apt-get -y install \
        curl \
        gnupg2 \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN curl -fsSL http://packages.gameap.ru/gameap-rep.gpg.key | apt-key add - \
    && echo "deb http://packages.gameap.ru/debian jessie main" > /etc/apt/sources.list.d/gameap.list

RUN apt-get -y update && apt-get -y install \
        gcc-multilib \
        g++-multilib \
        pkg-config \
        libbinn-dev \
        librestclientcpp-dev \
        libjsoncpp-dev \
        libssl-dev \
        libcurl4-openssl-dev \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN wget http://mirror.linux-ia64.org/gnu/gcc/releases/gcc-9.3.0/gcc-9.3.0.tar.gz \
    && tar -xvf gcc-9.3.0.tar.gz \
    && cd gcc-9.3.0 \
    && ./contrib/download_prerequisites \
    && mkdir objdir && cd objdir \
    && $PWD/../configure --prefix=/opt/gcc-9.3.0 \
    && make -j $(grep -c ^processor /proc/cpuinfo) \
    && make install \
    && update-alternatives --install /usr/bin/gcc gcc /opt/gcc-9.3.0/bin/gcc 60 --slave /usr/bin/g++ g++ /opt/gcc-9.3.0/bin/g++ \
    && cd ../.. && rm -r gcc-9.3.0 && rm gcc-9.3.0.tar.gz

# Download CMake
RUN wget https://cmake.org/files/v3.17/cmake-3.17.2-Linux-x86_64.tar.gz \
    && tar -xvf cmake-3.17.2-Linux-x86_64.tar.gz \
    && cp -R cmake-3.17.2-Linux-x86_64/* \
    && rm cmake-3.17.2-Linux-x86_64.tar.gz \
    && rm -rf rm cmake-3.17.2-Linux-x86_64

RUN wget http://packages.gameap.ru/others/boost_1_70_0.tar.gz \
    && tar -xvf boost_1_70_0.tar.gz \
    && cd boost_1_70_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,serialization,system,thread,test \
    && ./b2 install --link=static -j $(grep -c ^processor /proc/cpuinfo) \
    && cd .. && rm -r boost_1_70_0 && rm boost_1_70_0.tar.gz
