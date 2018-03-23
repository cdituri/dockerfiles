FROM alpine:latest

MAINTAINER Chris Dituri <csdituri@gmail.com>

# install privoxy
RUN apk add --no-cache privoxy

# privoxy bind_addr and debug run-time defaults
ARG PRIVOXY_BIND_ADDR

ENV PRIVOXY_BIND_ADDR ${PRIVOXY_BIND_ADDR:-0.0.0.0:8118}

ARG PRIVOXY_DEBUG_LEVEL

ENV PRIVOXY_DEBUG_LEVEL ${PRIVOXY_DEBUG_LEVEL:-0}

# /etc/privoxy/config base changes
RUN sed -i "s/^\(listen-address\).*/\1 ${PRIVOXY_BIND_ADDR}/" /etc/privoxy/config && \
    sed -i "s/^\(logfile .*\)/#\1/" /etc/privoxy/config && \
    echo "debug ${PRIVOXY_DEBUG_LEVEL}" >> /etc/privoxy/config

EXPOSE 8118

USER privoxy

ENTRYPOINT ["privoxy", "--no-daemon", "/etc/privoxy/config"]
