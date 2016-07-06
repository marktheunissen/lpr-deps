FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    build-essential \
    ca-certificates \
    rubygems-integration \
    ruby-dev \
    wget \
    gem install fpm \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/src
RUN wget -q -O leptonica.tar.gz https://github.com/DanBloomberg/leptonica/archive/v1.73.tar.gz \
    && tar zxf leptonica.tar.gz \
    && rm leptonica.tar.gz

WORKDIR /usr/local/src/leptonica-1.73
RUN chmod +x ./configure \
    && ./configure \
    && make -j4 \
    && make install
