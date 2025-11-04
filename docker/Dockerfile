FROM ubuntu:24.04

ENV DEBIAN_FRONTEND="noninteractive"
RUN apt-get update && apt-get install -y \
    build-essential \
    pkg-config \
    checkinstall \
    git \
    autoconf \
    automake \
    libtool-bin \
    libreadline-dev \
    libusb-1.0-0-dev \
    libcurl4-openssl-dev \
    libssl-dev \
    libzip-dev \
    zlib1g-dev \
    python3 \
    udev

RUN git clone https://github.com/libimobiledevice/libplist.git && \
    cd libplist && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libplist -rf

RUN git clone https://github.com/libimobiledevice/libtatsu.git && \
    cd libtatsu && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libtatsu -rf

RUN git clone https://github.com/libimobiledevice/libimobiledevice-glue.git && \
    cd libimobiledevice-glue && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libimobiledevice-glue -rf

RUN git clone https://github.com/libimobiledevice/libusbmuxd.git && \
    cd libusbmuxd && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libusbmuxd -rf

RUN git clone https://github.com/libimobiledevice/libimobiledevice.git && \
    cd libimobiledevice && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libimobiledevice -rf

RUN git clone https://github.com/libimobiledevice/libirecovery.git && \
    cd libirecovery && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm libirecovery -rf

RUN git clone https://github.com/libimobiledevice/usbmuxd.git  && \
    cd usbmuxd && \
    ./autogen.sh && \
    make && \
    make install && \
    cd .. && \
    rm usbmuxd -rf

# Build mceidevicerestore (your fork) with static linking
RUN git clone https://github.com/mceSystems/idevicerestore.git && \
    cd idevicerestore && \
    ./autogen.sh \
        libplist_CFLAGS=-DLIBPLIST_STATIC \
        limd_glue_CFLAGS=-DLIMD_GLUE_STATIC \
        enable_static=yes enable_shared=no \
        libtatsu_CFLAGS=-DLIBTATSU_STATIC \
        libusbmuxd_CFLAGS=-DLIBUSBMUXD_STATIC \
        libimobiledevice_CFLAGS=-DLIBIMOBILEDEVICE_STATIC \
        libcurl_CFLAGS=-DCURL_STATICLIB \
        libzip_CFLAGS=-DZIP_STATIC \
        LIBS="-lpsl -lssl -lcrypto -lssh2 -lnghttp2 -lnghttp3 -lzstd -lbrotlidec -lz -lidn2 -lbrotlicommon -lunistring -liconv -lintl -lbz2 -llzma" && \
    make && \
    make install && \
    cd .. && \
    rm idevicerestore -rf

RUN ldconfig
WORKDIR /tmp
COPY idevicerestore.sh /usr/sbin/idevicerestore.sh
CMD idevicerestore.sh

