# source buidler ----------------------

FROM alpine:latest as builder

MAINTAINER Chris Dituri <csdituri@gmail.com>

WORKDIR /build

ARG OPENCONNECT_VERSION

ENV OPENCONNECT_VERSION=${OPENCONNECT_VERSION:-7.08}
ENV VPNC_SCRIPTS_DATE=${VPNC_SCRIPTS_DATE:-20171005}

# gpg fingerprint is provided on the openconnect infradead.org homepage so we use it
ENV GPG_THUMBPRINT ${GPG_THUMBPRINT:-0xbe07d9fd54809ab2c4b0ff5f63762cda67e2f359}

# openconnect deps and gnu build system
RUN apk add --no-cache tar coreutils gnupg gcc make autoconf automake \
        libtool libxml2-dev linux-headers make musl-dev openssl-dev

# download, gpg verify openconnect, build pre-prep
RUN wget ftp://ftp.infradead.org/pub/openconnect/openconnect-${OPENCONNECT_VERSION}.tar.gz && \
    wget ftp://ftp.infradead.org/pub/openconnect/openconnect-${OPENCONNECT_VERSION}.tar.gz.asc && \
    gpg --receive-keys ${GPG_THUMBPRINT} && \
    gpg --verify openconnect-${OPENCONNECT_VERSION}.tar.gz.asc && \
    tar -xzpf openconnect-${OPENCONNECT_VERSION}.tar.gz

WORKDIR /build/openconnect-${OPENCONNECT_VERSION}

RUN wget ftp://ftp.infradead.org/pub/vpnc-scripts/vpnc-scripts-${VPNC_SCRIPTS_DATE}.tar.gz && \
    tar -xzpf vpnc-scripts-${VPNC_SCRIPTS_DATE}.tar.gz && \
    mkdir /etc/vpnc && \
    cp vpnc-scripts-${VPNC_SCRIPTS_DATE}/vpnc-script /etc/vpnc/ && \
    cp /etc/vpnc/vpnc-script /build/

# configure, make, install
RUN mkdir -p build/usr/local && \
    ./configure --prefix $(pwd)/build/usr/local --disable-nls && \
    make -j 4 && \
    make install && \
    cd build && \
    tar -cf /build/artifact.tar.gz .

WORKDIR /build

# minimal image ------------------------

FROM alpine:latest

MAINTAINER Chris Dituri <csdituri@gmail.com>

# openconnect deps
RUN apk add --no-cache libxml2 openssl && mkdir /etc/vpnc

COPY --from=builder /build/artifact.tar.gz /

COPY --from=builder /build/vpnc-script /etc/vpnc/vpnc-script

RUN tar -xf artifact.tar.gz && \
    rm artifact.tar.gz && \
    echo 'hosts: files dns' > /etc/nsswitch.conf

ENTRYPOINT ["openconnect"]
