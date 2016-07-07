FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    build-essential \
    cmake \
    git \
    pkg-config \
    ruby \
    ruby-dev \
    wget \
    && rm -rf /var/lib/apt/lists/

WORKDIR /usr/local/src
RUN gem install fpm \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/leptonica_1.73_armhf.deb \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/tesseract_3.04.01_armhf.deb \
    && dpkg -i leptonica_1.73_armhf.deb \
    && dpkg -i tesseract_3.04.01_armhf.deb \
    && rm leptonica_1.73_armhf.deb \
    && rm tesseract_3.04.01_armhf.deb

RUN wget -q -O opencv.tar.gz https://github.com/opencv/opencv/archive/3.1.0.tar.gz \
    && wget -q -O opencv_contrib.tar.gz https://github.com/opencv/opencv_contrib/archive/3.1.0.tar.gz \
    && tar zxf opencv.tar.gz && rm opencv.tar.gz \
    && tar zxf opencv_contrib.tar.gz && rm opencv_contrib.tar.gz

WORKDIR /usr/local/src/opencv-3.1.0/build
RUN cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_C_EXAMPLES=OFF \
        -D INSTALL_PYTHON_EXAMPLES=OFF \
        -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib-3.1.0/modules \
        -D BUILD_opencv_java=OFF \
        -D BUILD_opencv_python=OFF \
        -D BUILD_EXAMPLES=ON .. \
    && make -j4 \
    && mkdir -p /tmp/opencv \
    && make install DESTDIR=/tmp/opencv

WORKDIR /root/deb
RUN fpm -s dir \
    -t deb \
    -m "Mark Theunissen" \
    --url https://github.com/marktheunissen/lpr-deps \
    --description "OpenCV compiled for ARM" \
    -n opencv \
    -v 3.1.0 \
    -C /tmp/opencv \
    -p /root/deb/opencv_VERSION_ARCH.deb \
    usr

WORKDIR /root
