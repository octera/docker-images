FROM balenalib/raspberrypi4-64-debian-python:bullseye as runner

RUN install_packages \
    libgles2-mesa \
    lsb-release \
    mesa-vdpau-drivers \
    x11-xserver-utils \
    xserver-xorg-input-evdev \
    xserver-xorg-legacy \
    xserver-xorg-video-fbdev \
    xserver-xorg xinit \
    xinput xterm

RUN echo 'SUBSYSTEM=="vchiq",GROUP="video",MODE="0660"' > /etc/udev/rules.d/10-vchiq-permissions.rules

RUN install_packages \
    imagemagick

RUN pip3 install --no-cache-dir "kivy[base]" wand
