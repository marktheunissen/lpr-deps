FROM resin/rpi-raspbian:jessie

RUN apt-get update && apt-get install -y \
    wget \
    ca-certificates \
    libcurl4-openssl-dev \
    liblog4cplus-dev \
    && rm -rf /var/lib/apt/lists/

WORKDIR /usr/local/src
RUN wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/openalpr_2.2.0-c379120_armhf.deb \
&& wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/leptonica_1.73_armhf.deb \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/opencv_3.1.0_armhf.deb \
    && wget -q https://github.com/marktheunissen/lpr-deps/releases/download/v0.0.2/tesseract_3.04.01_armhf.deb \
    && dpkg -i opencv_3.1.0_armhf.deb \
    && dpkg -i leptonica_1.73_armhf.deb \
    && dpkg -i openalpr_2.2.0-c379120_armhf.deb \
    && dpkg -i tesseract_3.04.01_armhf.deb \
    && rm openalpr_2.2.0-c379120_armhf.deb \
    && rm opencv_3.1.0_armhf.deb \
    && rm tesseract_3.04.01_armhf.deb \
    && rm leptonica_1.73_armhf.deb \
    && ldconfig

WORKDIR /root
RUN wget -q http://plates.openalpr.com/ea7the.jpg

ENTRYPOINT alpr -c us ea7the.jpg
