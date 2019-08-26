FROM i386/buildpack-deps:stretch

RUN apt-get -y update && apt-get -y install \
        curl \
        gnupg2 \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN curl -fsSL http://packages.gameap.ru/gameap-rep.gpg.key | apt-key add - \
    && echo "deb http://packages.gameap.ru/debian stretch main" > /etc/apt/sources.list.d/gameap.list \
    && echo "deb http://ftp.de.debian.org/debian stretch-backports main " >> /etc/apt/sources.list

RUN apt-get -y update && apt-get -y install \
        cmake \
        pkg-config \
        libbinn-dev \
        librestclientcpp-dev \
        libjsoncpp-dev \
        libssl-dev \
        libcurl4-openssl-dev \
    --no-install-recommends && rm -r /var/lib/apt/lists/*

RUN apt-get -y update && apt-get -y -t stretch-backports install \
        libboost1.67-dev \
        libboost-system1.67-dev \
        libboost-filesystem1.67-dev \
        libboost-iostreams1.67-dev \
        libboost-thread1.67-dev \
        libboost-test1.67-dev \
    --no-install-recommends && rm -r /var/lib/apt/lists/*
