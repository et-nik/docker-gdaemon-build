FROM i386/buildpack-deps:bullseye

RUN apt-get -y update && apt-get -y install \
        curl \
        gnupg2 \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN curl -fsSL http://packages.gameap.ru/gameap-rep.gpg.key | apt-key add - \
    && echo "deb http://packages.gameap.ru/debian bullseye main" > /etc/apt/sources.list.d/gameap.list

RUN apt-get -y update && apt-get -y install \
        cmake \
        pkg-config \
        libboost-dev \
        libboost-system-dev \
        libboost-filesystem-dev \
        libboost-iostreams-dev \
        libboost-thread-dev \
        libboost-test-dev \
        libbinn-dev \
        librestclientcpp-dev \
        libjsoncpp-dev \
        libssl-dev \
        libcurl4-openssl-dev \
    --no-install-recommends && rm -r /var/lib/apt/lists/*
