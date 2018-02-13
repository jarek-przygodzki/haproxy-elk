FROM haproxy:1.8.4-alpine

RUN set -x && apk --update --allow-untrusted --virtual .build-deps \
        --repository http://dl-4.alpinelinux.org/alpine/edge/community \
        --arch=x86_64 add go git musl-dev && \
    GOPATH=/go CGO_ENABLED=0 go get -u github.com/ochinchina/supervisord && \
    mv /go/bin/supervisord /usr/bin/ && rm -rf /go && \
    apk del .build-deps &&\
    apk --update add rsyslog &&\
    rm -rf /var/cache/apk/* &&\
    mkdir /etc/supervisord.d &&\
    sed -i 's/#$ModLoad imudp/$ModLoad imudp/g' /etc/rsyslog.conf && \
    sed -i 's/#$UDPServerRun 514/$UDPServerRun 514/g' /etc/rsyslog.conf

COPY rsyslogd.d/ /etc/rsyslog.d/
COPY supervisord.d/ /etc/supervisord.d/
COPY supervisord.conf /etc/

ENTRYPOINT ["supervisord", "--configuration", "/etc/supervisord.conf"]
