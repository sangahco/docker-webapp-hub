FROM sangah/nginx-base

COPY ./index.html /usr/share/nginx/html/

ENV NGINX_VERSION=1.12.2

WORKDIR /usr/local/src

RUN set -e && \
  apk add --no-cache --virtual .build-deps \
    gcc \
    libc-dev \
    openssl-dev \
    pcre-dev \
    zlib-dev \
    linux-headers \
    curl \
    gnupg \
    libxslt-dev \
    gd-dev \
    geoip-dev \
    autoconf \
    automake \
    build-base \
    lmdb \
    libtool \
    make \
    openssl \
    ca-certificates \
    git && \
  update-ca-certificates && \
  git clone --depth 1 -b v3/master --single-branch https://github.com/SpiderLabs/ModSecurity && \
  cd ModSecurity && \
  git submodule init && \
  git submodule update && \
  ./build.sh && \
  ./configure && \
  make && \
  make install && \
  cd /usr/local/src && \
  git clone --depth 1 https://github.com/SpiderLabs/ModSecurity-nginx.git && \
  wget http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz && \
  tar zxvf nginx-${NGINX_VERSION}.tar.gz && \
  cd nginx-${NGINX_VERSION} && \
  ./configure --with-compat --add-dynamic-module=../ModSecurity-nginx && \
  make modules && \
  cp objs/ngx_http_modsecurity_module.so /etc/nginx/modules && \
  mkdir /etc/nginx/modsec && \
  wget -P /etc/nginx/modsec/ http://raw.githubusercontent.com/SpiderLabs/ModSecurity/v2/master/modsecurity.conf-recommended && \
  mv /etc/nginx/modsec/modsecurity.conf-recommended /etc/nginx/modsec/modsecurity.conf && \
  sed -i 's/SecRuleEngine DetectionOnly/SecRuleEngine On/' /etc/nginx/modsec/modsecurity.conf && \
  echo 'load_module modules/ngx_http_modsecurity_module.so;' >> /etc/nginx/nginx.template.conf && \
  # cleanup
  apk del .build-deps && \
  rm -rf /usr/local/src