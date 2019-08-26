FROM i386/buildpack-deps:wheezy

RUN apt-get -y update && apt-get -y install \
        curl \
        gnupg2 \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN curl -fsSL http://packages.gameap.ru/gameap-rep.gpg.key | apt-key add - \
    && echo "deb http://packages.gameap.ru/debian wheezy main" > /etc/apt/sources.list.d/gameap.list

RUN apt-get -y update && apt-get -y install \
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
    
RUN wget https://cmake.org/files/v3.10/cmake-3.10.3-Linux-x86_64.tar.gz \
    && tar -xvf cmake-3.10.3-Linux-x86_64.tar.gz \
    && cp -R cmake-3.10.3-Linux-x86_64/* / \
    && rm -r cmake-3.10.3-Linux-x86_64 \
    && rm cmake-3.10.3-Linux-x86_64.tar.gz
