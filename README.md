# OpenALPR ARM Docker Build

Build OpenALPR and all its dependencies for ARM architecture using Docker.


## Releases

The .deb files are available on the [releases page](https://github.com/marktheunissen/lpr-deps/releases). Download them and then do:

    dpkg -i <file.deb>


## Building yourself

Either use a Raspberry Pi or fire up a Scaleway ARM cloud VM to build. You then can install the latest Docker version which is packaged by Hypriot:

    wget https://downloads.hypriot.com/docker-hypriot_1.10.3-1_armhf.deb
    sudo dpkg -i docker-hypriot_1.10.3-1_armhf.deb
    systemctl start docker

Then, build them in order, which is:

1. leptonica
2. tesseract
3. opencv
4. openalpr

You'll have to modify each Dockerfile to point to your newly built intermediate .deb files.

    cd leptonica / tesseract / etc.
    make build
    make deb


## Notes

This uses `fpm` to build deb files, see:

- https://github.com/jordansissel/fpm/wiki/PackageMakeInstall
