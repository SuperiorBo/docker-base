FROM alpine:3.15
LABEL MAINTAINER=chobon@aliyun.com

# Add s6-overlay
ENV S6_OVERLAY_VERSION=v2.1.0.2 \
    GO_DNSMASQ_VERSION=1.0.7 \
    TIME_ZONE=Asia/Shanghai

# change ALIYUN apk source
#RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

# create user and give binary permissions to bind to lower port
RUN apk add --update --no-cache tzdata bind-tools curl libcap && \
    curl -sSL https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
    | tar xfz - -C / && \
    # curl -R -L -o /tmp/dist.zip https://nightly.link/just-containers/s6-overlay/workflows/all/issue-329/dist.zip && \
    # cd /tmp && unzip dist.zip && chmod +x s6-overlay-amd64-installer && /tmp/s6-overlay-amd64-installer / && \
    curl -sSL https://github.com/janeczku/go-dnsmasq/releases/download/${GO_DNSMASQ_VERSION}/go-dnsmasq-min_linux-amd64 -o /bin/go-dnsmasq && \
    chmod +x /bin/go-dnsmasq && \
    apk del curl && \
    # create user and give binary permissions to bind to lower port
    addgroup go-dnsmasq && \
    adduser -D -g "" -s /bin/sh -G go-dnsmasq go-dnsmasq && \
    setcap CAP_NET_BIND_SERVICE=+eip /bin/go-dnsmasq

RUN ln -sf /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime && \
    echo "${TIME_ZONE}" > /etc/timezone

ADD root /

ENTRYPOINT ["/init"]
CMD []