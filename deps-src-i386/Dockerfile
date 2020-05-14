FROM i386/buildpack-deps:buster

RUN apt-get -y update && apt-get -y install --reinstall \
        curl \
        gnupg2 \
	libc6 \
        libc6-dev \
	gcc-multilib \
        g++-multilib \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

# Download and compile CMake
RUN wget https://cmake.org/files/v3.17/cmake-3.17.2.tar.gz \
    && tar -xvf cmake-3.17.2.tar.gz \
    && cd cmake-3.17.2 \
    && ./configure \
    && make -j$(grep -c ^processor /proc/cpuinfo) install \
    && cd .. && rm -r cmake-3.17.2

# Compile Boost
RUN wget http://packages.gameap.ru/others/boost_1_67_0.tar.gz \
    && tar -xvf boost_1_67_0.tar.gz \
    && cd boost_1_67_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,serialization,system,thread,test \
    && ./b2 install --link=static -j $(grep -c ^processor /proc/cpuinfo) \
    && cd .. && rm -r boost_1_67_0 && rm boost_1_67_0.tar.gz

# Compile Binn
RUN git clone https://github.com/liteserver/binn \
    && cd binn \
    && make -j$(grep -c ^processor /proc/cpuinfo) \
    && ar rcs libbinn.a binn.o \
    && cp libbinn.a /usr/lib/libbinn.a \
    && cp src/binn.h /usr/include/binn.h \
    && cd .. && rm -r binn

# OpenSSL
RUN wget https://github.com/openssl/openssl/archive/OpenSSL_1_1_1d.tar.gz \
    && tar -xvf OpenSSL_1_1_1d.tar.gz \
    && cd openssl-OpenSSL_1_1_1d \
    && setarch i386 ./config -m32 \
    && make -j$(grep -c ^processor /proc/cpuinfo)\
    && make install \
    && cd .. && rm -r openssl-OpenSSL_1_1_1d && rm OpenSSL_1_1_1d.tar.gz

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
    && git checkout tags/0.5.1 \
    && ./autogen.sh \
    && ./configure \
    && make -j$(grep -c ^processor /proc/cpuinfo) \
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