FROM alpine:3.8

MAINTAINER Vincent Composieux <vincent.composieux@gmail.com>

RUN rm -rf /var/cache/apk/* && \
    rm -rf /tmp/*

RUN apk update

RUN apk add --update nginx
RUN rm -rf /var/cache/apk/* && rm -rf /tmp/*

ADD nginx.conf /etc/nginx/
ADD upstream.conf /etc/nginx/default/
ADD symfony.conf /etc/nginx/default/

RUN adduser -D -g '' -G www-data www-data

CMD ["nginx"]

EXPOSE 80
EXPOSE 443
