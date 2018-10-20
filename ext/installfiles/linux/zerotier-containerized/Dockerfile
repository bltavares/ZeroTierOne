FROM debian:stretch as builder

## Supports x86_64, x86, arm, and arm64

RUN apt-get update && apt-get install -y curl gnupg
RUN curl -s 'https://pgp.mit.edu/pks/lookup?op=get&search=0x1657198823E52A61' | gpg --import && \
    echo "deb ${ZT_BASE_URL_HTTP}debian/stretch stretch main" > /etc/apt/sources.list.d/zerotier.list && \
    apt-get update && apt-get install zerotier-one=1.2.8
    
FROM alpine:latest
MAINTAINER Adam Ierymenko <adam.ierymenko@zerotier.com>

LABEL version="1.2.8"
LABEL description="Containerized ZeroTier One for use on CoreOS or other Docker-only Linux hosts."

# Uncomment to build in container
#RUN apk add --update alpine-sdk linux-headers

RUN apk add --update libgcc libstdc++

RUN mkdir -p /var/lib/zerotier-one

COPY --from builder /var/lib/zerotier-one/zerotier-cli /usr/sbin/zerotier-cli
COPY --from builder /var/lib/zerotier-one/zerotier-idtool /usr/sbin/zerotier-idtool
COPY --from builder /usr/sbin/zerotier-one /usr/sbin/zerotier-one

ENTRYPOINT ["/usr/sbin/zerotier-one"]
