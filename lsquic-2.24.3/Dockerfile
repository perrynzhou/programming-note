FROM ubuntu:16.04

RUN apt-get update && \
    apt-get install -y build-essential git cmake software-properties-common \
                       zlib1g-dev libevent-dev

RUN add-apt-repository ppa:gophers/archive && \
    apt-get update && \
    apt-get install -y golang-1.9-go && \
    cp /usr/lib/go-1.9/bin/go* /usr/bin/.

RUN mkdir /src
WORKDIR /src

RUN mkdir /src/lsquic
COPY ./ /src/lsquic/

RUN git clone https://boringssl.googlesource.com/boringssl && \
    cd boringssl && \
    git checkout b117a3a0b7bd11fe6ebd503ec6b45d6b910b41a1 && \
    cmake . && \
    make

RUN cd /src/lsquic && \
    cmake -DBORINGSSL_DIR=/src/boringssl . && \
    make

RUN cd lsquic && make test && cp http_client /usr/bin/ && cp http_server /usr/bin
