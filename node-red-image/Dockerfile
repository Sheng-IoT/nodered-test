#### Use nodered/node-red:2.2.2 as the base
FROM nodered/node-red:2.2.2 AS base

USER root
# Change to tsinghua mirror, comment out if you are not in China mainland
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.tuna.tsinghua.edu.cn/g' /etc/apk/repositories 

# Copy package.json contains curated version of Node-RED NPM module and node dependencies
COPY package.json .
# COPY start-pigpiod /etc/init.d
# COPY pigpiod.service /etc/init.d/

# Install node-red nodes
RUN npm install --unsafe-perm --no-update-notifier --no-fund --only=production

# Install pigpio following the guide on http://abyz.me.uk/rpi/pigpio/download.html
RUN apk --update --no-cache add --virtual build-dependencies build-base && \
    apk add --no-cache openrc && \
    cd /tmp && \
    wget https://github.com/joan2937/pigpio/archive/master.zip && \
    unzip -qq master.zip && \
    cd pigpio-master && \
    make && \
    sed -i 's/ldconfig/ldconfig \/usr\/local/g' Makefile && \
    make install && \
    apk del build-dependencies && \
    rm -rf /tmp/* && \
    apk --no-cache add tini 

# Install systemd
#RUN cd /tmp && git clone https://github.com/systemd/systemd

#RUN echo "unicode=\"YES\"" >> /etc/rc.conf && \
#    apk add --no-cache --virtual .build_deps \
#        autoconf file g++ gcc libc-dev make pkgconf python3 ninja \
#        util-linux pciutils usbutils coreutils binutils findutils grep \
#        build-base gcc abuild binutils binutils-doc gcc-doc gperf libcap libcap-dev \
#        valgrind-dev \
#    && \
#    pip3 install meson

#RUN cd /tmp/systemd && \
#    meson build && \
#    ninja build


COPY start-pigpiod.start /etc/local.d/start-pigpiod.start

#RUN update-rc.d pigpiod defaults

RUN mkdir -p /run/openrc && \
    touch /run/openrc/softlevel && \
    rc-update add local default 
#    rc-update add start-pigpiod default 
#    rc-service start-pigpiod start
#    rc-update add local
 
#CMD ["bash", "./start.sh"]
USER node-red
 
