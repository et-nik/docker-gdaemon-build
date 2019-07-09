FROM tcnksm/centos-buildpack-deps:7

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
