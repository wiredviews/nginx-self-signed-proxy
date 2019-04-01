FROM alpine:3.9 as builder
RUN apk add --no-cache openssl
RUN openssl req \
            -x509 \
            -subj "/C=US/ST=Denial/L=Nowhere/O=Dis" \
            -nodes \
            -days 3650 \
            -newkey rsa:2048 \
            -keyout nginx.key \
            -out nginx.cert

FROM nginx:1.15.10

RUN mkdir /etc/nginx/ssl 

COPY ./conf.d/*.conf /etc/nginx/conf.d
COPY --from=builder nginx.key /etc/nginx/ssl
COPY --from=builder nginx.cert /etc/nginx/ssl

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]