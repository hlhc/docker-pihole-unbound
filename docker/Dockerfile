FROM pihole/pihole:2026.02.0
RUN apk update && apk add unbound

ARG S6_OVERLAY_VERSION=3.2.2.0
ARG PIHOLE_UID=1000
ARG PIHOLE_GID=1000

RUN apk update && apk add unbound \
    && mkdir -p /var/log/unbound \
    && chown unbound /var/log/unbound

ENV DNSMASQ_USER=pihole

COPY lighttpd-external.conf /etc/lighttpd/external.conf
COPY unbound-pihole.conf /etc/unbound/unbound.conf.d/pi-hole.conf
COPY 99-edns.conf /etc/dnsmasq.d/99-edns.conf

ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-noarch.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-noarch.tar.xz
ADD https://github.com/just-containers/s6-overlay/releases/download/v${S6_OVERLAY_VERSION}/s6-overlay-x86_64.tar.xz /tmp
RUN tar -C / -Jxpf /tmp/s6-overlay-x86_64.tar.xz

COPY s6-overlay/s6-rc.d/unbound /etc/s6-overlay/s6-rc.d/unbound
COPY s6-overlay/s6-rc.d/pihole /etc/s6-overlay/s6-rc.d/pihole
COPY s6-overlay/s6-rc.d/user/contents.d /etc/s6-overlay/s6-rc.d/user/contents.d
RUN chmod +x /etc/s6-overlay/s6-rc.d/unbound/run \
    && chmod +x /etc/s6-overlay/s6-rc.d/pihole/run \
    && chmod +x /etc/s6-overlay/s6-rc.d/pihole/finish

COPY resolv.conf /etc/resolv.conf

ENTRYPOINT ["/init"]
