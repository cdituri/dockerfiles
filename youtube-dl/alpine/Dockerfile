FROM alpine:latest

MAINTAINER Chris Dituri <csdituri@gmail.com>

RUN apk add --no-cache youtube-dl aria2

ADD youtube-dl.conf /etc/youtube-dl.conf

WORKDIR /data

ENTRYPOINT ["youtube-dl"]
