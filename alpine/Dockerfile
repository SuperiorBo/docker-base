FROM alpine:3.9
LABEL MAINTAINER=chobon@aliyun.com

# Add s6-overlay
ENV S6_OVERLAY_VERSION=v1.22.1.0 \
    GO_DNSMASQ_VERSION=1.0.7

# change ALIYUN apk source
#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

ADD https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz /tmp/ 
RUN gunzip -c /tmp/s6-overlay-amd64.tar.gz | tar -xf - -C / && rm -rf /tmp/s6-overlay-amd64.tar.gz

ADD https://github.com/janeczku/go-dnsmasq/releases/download/${GO_DNSMASQ_VERSION}/go-dnsmasq-min_linux-amd64 /go-dnsmasq
RUN chmod +x /bin/go-dnsmasq

# RUN apk add --update --no-cache tzdata xz bind-tools curl libcap && \
#     curl -SL https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
#     | tar xfz - -C / && \
#     curl -SL https://github.com/janeczku/go-dnsmasq/releases/download/${GO_DNSMASQ_VERSION}/go-dnsmasq-min_linux-amd64 -o /bin/go-dnsmasq && \
#     chmod +x /bin/go-dnsmasq && \
#     apk del curl && \
    # create user and give binary permissions to bind to lower port
RUN addgroup go-dnsmasq && \
    adduser -D -g "" -s /bin/sh -G go-dnsmasq go-dnsmasq && \
    setcap CAP_NET_BIND_SERVICE=+eip /bin/go-dnsmasq

ADD root /

ENTRYPOINT ["/init"]
CMD []