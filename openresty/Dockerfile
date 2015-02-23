FROM debian:wheezy
MAINTAINER catatsuy@catatsuy.org

# aptで日本のミラーを使う
RUN echo "deb http://cdn.debian.or.jp/debian wheezy main" > /etc/apt/sources.list && \
  echo "deb http://cdn.debian.or.jp/debian wheezy-updates main" >> /etc/apt/sources.list && \
  echo "deb http://security.debian.org/ wheezy/updates main" >> /etc/apt/sources.list

ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get upgrade -y && apt-get clean && \
  apt-get -y install wget perl make libreadline-dev libncurses5-dev libpcre3-dev libssl-dev && \
  rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# OpenResty (Nginx)
RUN wget http://openresty.org/download/ngx_openresty-1.7.7.2.tar.gz \
  && tar xvfz ngx_openresty-1.7.7.2.tar.gz \
  && cd ngx_openresty-1.7.7.2 \
  && ./configure --with-luajit --with-http_addition_module --with-http_gzip_static_module --with-http_realip_module --with-http_stub_status_module --with-http_sub_module --with-ipv6 --with-pcre-jit \
  && make \
  && make install \
  && rm -rf /ngx_openresty*

VOLUME ["/var/log/nginx"]

WORKDIR /usr/local/openresty/nginx/

# COPY nginx.conf /usr/local/openresty/nginx/conf/nginx.conf

EXPOSE 80

CMD ["/usr/local/openresty/nginx/sbin/nginx","-c","conf/nginx.conf"]
