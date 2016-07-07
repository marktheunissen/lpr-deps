FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    pkg-config \
    ruby \
    ruby-dev \
    wget \
    liblog4cplus-dev \
    libcurl4-openssl-dev \
    && rm -rf /var/lib/apt/lists/

WORKDIR /usr/local/src
RUN gem install fpm \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/leptonica_1.73_armhf.deb \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/tesseract_3.04.01_armhf.deb \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/opencv_3.1.0_armhf.deb \
    && dpkg -i leptonica_1.73_armhf.deb \
    && dpkg -i tesseract_3.04.01_armhf.deb \
    && dpkg -i opencv_3.1.0_armhf.deb \
    && rm opencv_3.1.0_armhf.deb \
    && rm leptonica_1.73_armhf.deb \
    && rm tesseract_3.04.01_armhf.deb

RUN git clone --depth 1 https://github.com/openalpr/openalpr.git

WORKDIR /usr/local/src/openalpr/src
RUN sed -i '11 i SET(Tesseract_DIR "/usr/local")' /usr/local/src/openalpr/src/CMakeLists.txt \
    && sed -i '11 i SET(OpenCV_DIR "/usr/local/lib")' /usr/local/src/openalpr/src/CMakeLists.txt \
    && cmake ./ \
    && make -j4 \
    && make install DESTDIR=/tmp/openalpr

WORKDIR /root/deb
RUN echo "export OPENALPR_VERSION=2.2.0-`cd /usr/local/src/openalpr; git rev-parse --short HEAD`" > ENV_VARS \
    && . ./ENV_VARS \
    && rm ENV_VARS \
    && fpm -s dir \
    -t deb \
    -m "Mark Theunissen" \
    --url https://github.com/marktheunissen/lpr-deps \
    --description "OpenALPR compiled for ARM" \
    -n openalpr \
    -v $OPENALPR_VERSION \
    -C /tmp/openalpr \
    -p /root/deb/openalpr_VERSION_ARCH.deb \
    usr \
    etc

WORKDIR /root
