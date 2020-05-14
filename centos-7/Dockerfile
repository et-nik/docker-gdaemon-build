FROM knik/centos-buildpack-deps:7

RUN yum -y install glibc-devel.i686 libgcc.i686 libstdc++-devel.i686 ncurses-devel.i686

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
RUN wget http://packages.gameap.ru/others/boost_1_67_0.tar.gz \
    && tar -xvf boost_1_67_0.tar.gz \
    && cd boost_1_67_0 \
    && ./bootstrap.sh --with-libraries=chrono,exception,filesystem,iostreams,system \
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

# Compile Rest Client Library
RUN git clone https://github.com/mrtazz/restclient-cpp \
    && cd restclient-cpp \
    && git checkout tags/0.5.0 \
    && ./autogen.sh \
    && ./configure --enable-static=yes --enable-shared=no \
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
