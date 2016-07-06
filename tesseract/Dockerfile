FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    build-essential \
    automake \
    libtool \
    ca-certificates \
    ruby \
    ruby-dev \
    wget \
    && rm -rf /var/lib/apt/lists/

WORKDIR /usr/local/src
RUN wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.1/leptonica_1.73_armhf.deb \
    && dpkg -i leptonica_1.73_armhf.deb

RUN wget -q -O tesseract.tar.gz https://github.com/tesseract-ocr/tesseract/archive/3.04.01.tar.gz \
    && wget -q -O tessdata.tar.gz https://tesseract-ocr.googlecode.com/files/tesseract-ocr-3.02.eng.tar.gz \
    && tar zxf tesseract.tar.gz && rm tesseract.tar.gz \
    && tar zxf tessdata.tar.gz && rm tessdata.tar.gz

ENV TESSDATA_PREFIX /usr/local/src/tesseract-ocr/tessdata
WORKDIR /usr/local/src/tesseract-3.04.01
RUN ./autogen.sh \
    && ./configure \
    && make -j4 \
    && mkdir -p /tmp/tesseract \
    && make install DESTDIR=/tmp/tesseract

RUN fpm -s dir \
    -t deb \
    -m "Mark Theunissen" \
    --url https://github.com/marktheunissen/lpr-deps \
    --description "Tesseract compiled for ARM" \
    -n tesseract \
    -v 3.04.01 \
    -C /tmp/tesseract \
    -p /root/tesseract_VERSION_ARCH.deb \
    usr
