FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    build-essential \
    ca-certificates \
    ruby \
    ruby-dev \
    wget \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/src
RUN gem install fpm \
    && wget -q -O leptonica.tar.gz https://github.com/DanBloomberg/leptonica/archive/v1.73.tar.gz \
    && tar zxf leptonica.tar.gz \
    && rm leptonica.tar.gz

WORKDIR /usr/local/src/leptonica-1.73
RUN mkdir -p /tmp/leptonica \
    && chmod +x ./configure \
    && ./configure \
    && make -j4 \
    && make install DESTDIR=/tmp/leptonica

RUN fpm -s dir \
    -t deb \
    -m "Mark Theunissen" \
    --url https://github.com/marktheunissen/lpr-deps \
    --description "Leptonica compiled for ARM" \
    -n leptonica \
    -v 1.73 \
    -C /tmp/leptonica \
    -p /root/leptonica_VERSION_ARCH.deb \
    usr
