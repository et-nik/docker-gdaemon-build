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
        libssl-dev \
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
RUN cd /opt \
    && wget https://cmake.org/files/v3.17/cmake-3.17.2-Linux-x86_64.tar.gz \
    && tar -xvf cmake-3.17.2-Linux-x86_64.tar.gz \
    && rm cmake-3.17.2-Linux-x86_64.tar.gz \
    && update-alternatives --install /usr/bin/cmake cmake /opt/cmake-3.17.2-Linux-x86_64/bin/cmake 60 \
        --slave /usr/bin/cmake-gui cmake-gui /opt/cmake-3.17.2-Linux-x86_64/bin/cmake-gui \
        --slave /usr/bin/ccmake ccmake /opt/cmake-3.17.2-Linux-x86_64/bin/ccmake \
        --slave /usr/bin/ctest ctest /opt/cmake-3.17.2-Linux-x86_64/bin/ctest \
        --slave /usr/bin/cpack cpack /opt/cmake-3.17.2-Linux-x86_64/bin/cpack

# Compile Boost
RUN wget http://packages.gameap.ru/others/boost_1_70_0.tar.gz \
    && tar -xvf boost_1_70_0.tar.gz \
    && cd boost_1_70_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,system \
    && ./b2 install --link=static -j $(grep -c ^processor /proc/cpuinfo) \
    && cd .. && rm -r boost_1_70_0 && rm boost_1_70_0.tar.gz

# Compile Curl Library
RUN wget https://curl.haxx.se/download/curl-7.60.0.tar.gz \
    && tar -xvf curl-7.60.0.tar.gz \
    && cd curl-7.60.0 \
    && mkdir build && cd build \
    && cmake -DCURL_STATICLIB=ON -DBUILD_SHARED_LIBS=OFF -DCMAKE_USE_OPENSSL=ON .. \
    && cmake --build . \
    && make install \
    && cd ../.. && rm curl-7.60.0.tar.gz && rm -rf curl-7.60.0

# Compile Rest Client Library
RUN git clone https://github.com/mrtazz/restclient-cpp \
    && cd restclient-cpp \
    && git checkout tags/0.5.0 \
    && ./autogen.sh \
    && ./configure \
    && make -j $(grep -c ^processor /proc/cpuinfo) \
    && make install \
    && cd .. && rm -rf restclient-cpp

# Compile Json Cpp
RUN git clone https://github.com/open-source-parsers/jsoncpp \
    && cd jsoncpp \
    && git checkout tags/1.8.4 \
    && mkdir build && cd build \
    && cmake -DJSONCPP_WITH_TESTS=OFF -DCMAKE_BUILD_TYPE=release -DBUILD_STATIC_LIBS=ON -DBUILD_SHARED_LIBS=OFF .. \
    && cmake --build . \
    && make install \
    && mkdir /usr/local/include/jsoncpp \
    && mv /usr/local/include/json /usr/local/include/jsoncpp/json \
    && cd ../.. && rm -rf jsoncpp