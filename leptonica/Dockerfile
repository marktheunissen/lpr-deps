FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    git \
    automake \
    libtool \
    cmake \
    pkg-config \
    python-pip \
    python2.7-dev \
    libcurl4-openssl-dev \
    liblog4cplus-1.0-4 \
    liblog4cplus-dev \
    uuid-dev \
    autoconf \
    build-essential \
    ca-certificates \
    curl \
    wget \
    && rm -rf /var/lib/apt/lists/*

WORKDIR /usr/local/src
RUN git clone --depth 1 https://github.com/openalpr/openalpr.git \
    && wget -q -O tesseract.tar.gz https://github.com/tesseract-ocr/tesseract/archive/3.04.01.tar.gz \
    && wget -q -O tessdata.tar.gz https://github.com/tesseract-ocr/tessdata/archive/3.04.00.tar.gz \
    && wget -q -O leptonica.tar.gz https://github.com/DanBloomberg/leptonica/archive/v1.73.tar.gz \
    && wget -q -O opencv.tar.gz https://github.com/opencv/opencv/archive/3.1.0.tar.gz \
    && wget -q -O opencv_contrib.tar.gz https://github.com/opencv/opencv_contrib/archive/3.1.0.tar.gz \
    && tar zxf tesseract.tar.gz && rm tesseract.tar.gz \
    && tar zxf tessdata.tar.gz && rm tessdata.tar.gz \
    && tar zxf leptonica.tar.gz && rm leptonica.tar.gz \
    && tar zxf opencv.tar.gz && rm opencv.tar.gz \
    && tar zxf opencv_contrib.tar.gz && rm opencv_contrib.tar.gz

WORKDIR /usr/local/src/leptonica-1.73
RUN chmod +x ./configure \
    && ./configure \
    && make -j4 \
    && make install

ENV TESSDATA_PREFIX /usr/local/src/tessdata-3.04.00
WORKDIR /usr/local/src/tesseract-3.04.01
RUN ./autogen.sh \
    && ./configure \
    && make -j4 \
    && make install \
    && ldconfig

WORKDIR /usr/local/src/opencv-3.1.0/build
RUN pip install numpy \
    && cmake -D CMAKE_BUILD_TYPE=RELEASE \
        -D CMAKE_INSTALL_PREFIX=/usr/local \
        -D INSTALL_C_EXAMPLES=OFF \
        -D INSTALL_PYTHON_EXAMPLES=ON \
        -D OPENCV_EXTRA_MODULES_PATH=/usr/local/src/opencv_contrib-3.1.0/modules \
        -D BUILD_EXAMPLES=ON .. \
    && make -j4 \
    && make install \
    && ldconfig

WORKDIR /usr/local/src/openalpr/src
RUN sed -i '11 i SET(Tesseract_DIR "/usr/local/src/tesseract-3.04.01")' /usr/local/src/openalpr/src/CMakeLists.txt \
    && sed -i '11 i SET(OpenCV_DIR "/usr/local/src/opencv-3.1.0/build")' /usr/local/src/openalpr/src/CMakeLists.txt \
    && cmake ./ \
    && make -j4 \
    && make install \
    && ldconfig
