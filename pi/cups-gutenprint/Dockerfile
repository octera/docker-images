FROM balenalib/raspberrypi4-64-debian:bullseye as base

RUN install_packages \
    avahi-daemon \
    libusb-1.0-0 libcups2 \
    cups

FROM base as builder
# Install the packages we need. Avahi will be included
RUN install_packages \
    iptables \
    avahi-daemon \
    libusb-1.0-0-dev libcups2-dev \
    build-essential \
    git \
    pkg-config \
    checkinstall

RUN mkdir -p /build
WORKDIR /build

RUN curl -o gutenprint-5.3.4-2021-10-15T01-00-0b8c6c6e.tar.xz \
    "https://master.dl.sourceforge.net/project/gimp-print/snapshots/gutenprint-5.3.4-2021-10-15T01-00-0b8c6c6e.tar.xz?viasf=1"
RUN tar -xJf gutenprint-5.3.4-2021-10-15T01-00-0b8c6c6e.tar.xz
RUN cd gutenprint-5.3.4-2021-10-15T01-00-0b8c6c6e && \
    ./configure --without-doc && \
    make -j4 && \
    checkinstall --fstrans=no --default

RUN ls -la

WORKDIR /build

RUN git clone --depth 1 https://gitlab.com/speachy/selphy_print.git && \
    cd selphy_print && \
    make -j4 && \
    checkinstall --install=no --default

RUN ls -lah selphy_print

FROM base as final

COPY --from=builder /build/gutenprint-5.3.4-2021-10-15T01-00-0b8c6c6e/gutenprint-5.3.4-2021-10-15t01-00_0b8c6c6e-1_arm64.deb /
COPY --from=builder /build/selphy_print/selphy-print_*-1_arm64.deb /

RUN dpkg -i /gutenprint-5.3.4-2021-10-15t01-00_0b8c6c6e-1_arm64.deb && \
    dpkg -i --force-overwrite /selphy-print_*-1_arm64.deb && \
    rm /*arm64.deb


RUN cups-genppdupdate
RUN echo "/usr/local/lib" > /etc/ld.so.conf.d/usr-local.conf
RUN ldconfig
