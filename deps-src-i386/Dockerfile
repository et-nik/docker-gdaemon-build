FROM i386/buildpack-deps:buster

RUN apt-get -y update && apt-get -y install --reinstall \
        curl \
        gnupg2 \
	libc6 \
        libc6-dev \
	gcc-multilib \
        g++-multilib \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

# Download CMake
RUN wget https://cmake.org/files/v3.10/cmake-3.10.3-Linux-x86_64.tar.gz \
    && tar -xvf cmake-3.10.3-Linux-x86_64.tar.gz \
    && cp -R cmake-3.10.3-Linux-x86_64/* / \
    && rm -r cmake-3.10.3-Linux-x86_64 && rm cmake-3.10.3-Linux-x86_64.tar.gz

# Compile Boost
RUN wget http://packages.gameap.ru/others/boost_1_70_0.tar.gz \
    && tar -xvf boost_1_70_0.tar.gz \
    && cd boost_1_70_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,serialization,system,thread,test \
    && ./b2 install --link=static -j $(grep -c ^processor /proc/cpuinfo) \
    && cd .. && rm -r boost_1_70_0 && rm boost_1_70_0.tar.gz

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
#    && ln -s /usr/local/lib64/libssl.so.1.1 /usr/lib64/libssl.so.1.1 \
#    && ln -s /usr/local/lib64/libcrypto.so.1.1 /usr/lib64/libcrypto.so.1.1 \
#    && ln -s /usr/local/lib64/libssl.a /usr/lib64/libssl.a \
#    && ln -s /usr/local/lib64/libcrypto.a /usr/lib64/libcrypto.a \
    && cd .. && rm -r openssl-OpenSSL_1_1_1d && rm OpenSSL_1_1_1d.tar.gz

# Compile Curl Library
RUN wget https://github.com/curl/curl/releases/download/curl-7_66_0/curl-7.66.0.tar.gz  \
    && tar -xvf curl-7.66.0.tar.gz  \
    && cd curl-7.66.0  \
    && ./configure --with-ssl --disable-shared \
    && make -j$(grep -c ^processor /proc/cpuinfo)\
    && make install \
    && cd .. && rm -r curl-7.66.0 && rm curl-7.66.0.tar.gz

# Compile Rest Client Library
RUN git clone https://github.com/mrtazz/restclient-cpp \
    && cd restclient-cpp \
    && git checkout tags/0.5.1 \
    && ./autogen.sh \
    && ./configure \
    && make -j$(grep -c ^processor /proc/cpuinfo)\
    && make install \
    && cd .. && rm -r restclient-cpp

# Compile Json Cpp
RUN git clone https://github.com/open-source-parsers/jsoncpp \
    && cd jsoncpp \
    && git checkout tags/1.8.4 \
    && cmake . \
    && make -j$(grep -c ^processor /proc/cpuinfo)\
    && make install \
    && mkdir /usr/local/include/jsoncpp \
    && mv /usr/local/include/json /usr/local/include/jsoncpp/json \
#    && ln -s /usr/local/lib64/libjsoncpp.a /usr/lib64/libjsoncpp.a
    && cd .. && rm -r jsoncpp
