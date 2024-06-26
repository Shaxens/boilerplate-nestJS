FROM nginx:1.24.0

RUN apt-get update && apt-get install -y wget

ENV NGINX_VERSION="1.24.0"

RUN wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -O nginx.tar.gz

RUN apt-get update && apt-get install -y

# Build dependencies for nginx
# RUN echo "deb-src http://deb.debian.org/debian bullseye main" >> /etc/apt/sources.list
# RUN apt-get update
# RUN apt-get build-dep nginx -y

# RUN apt-get install linux-headers-$(uname -r)

RUN apt-get update && apt-get install -y \
  gcc \
  libc-dev \
  make \
  libssl-dev \
  libpcre3 \
  libpcre3-dev \
  # pcre-dev \
  zlib1g-dev \
  # zlib-dev \
  linux-headers-generic \
  curl \
  gnupg \
  libxslt-dev \
  libgd-dev \
  # gd-dev \
  libgeoip-dev \
  # geoip-dev \
  git \
  patch

ENV CONFARGS --prefix=/etc/nginx --sbin-path=/usr/sbin/nginx --modules-path=/usr/lib/nginx/modules --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path=/var/log/nginx/access.log --pid-path=/var/run/nginx.pid --lock-path=/var/run/nginx.lock --http-client-body-temp-path=/var/cache/nginx/client_temp --http-proxy-temp-path=/var/cache/nginx/proxy_temp --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp --http-scgi-temp-path=/var/cache/nginx/scgi_temp --with-perl_modules_path=/usr/lib/perl5/vendor_perl --user=nginx --group=nginx --with-compat --with-file-aio --with-threads --with-http_addition_module --with-http_auth_request_module --with-http_dav_module --with-http_flv_module --with-http_gunzip_module --with-http_gzip_static_module --with-http_mp4_module --with-http_random_index_module --with-http_realip_module --with-http_secure_link_module --with-http_slice_module --with-http_ssl_module --with-http_stub_status_module --with-http_sub_module --with-http_v2_module --with-mail --with-mail_ssl_module --with-stream --with-stream_realip_module --with-stream_ssl_module --with-stream_ssl_preread_module --with-cc-opt='-Os' --with-ld-opt=-Wl,--as-needed,-O1,--sort-common

RUN mkdir -p /usr/src

RUN tar -zxC /usr/src -f nginx.tar.gz && \
    cd /usr/src/nginx-$NGINX_VERSION && \
    git clone --depth 1 --single-branch --branch master https://github.com/nginx-modules/nginx_upstream_check_module && \
    patch -p1 < nginx_upstream_check_module/check_1.16.1+.patch && \
    ./configure $CONFARGS --add-module=nginx_upstream_check_module && \
    make && make install && \
    cd / && \
    # apt-get remove .build-deps && \
    rm -rf /usr/src

COPY loadbalancer/conf.d/loadbalancer.conf /etc/nginx/conf.d/

COPY loadbalancer/include/proxy.conf /etc/nginx/conf.d/

COPY nginx.conf /etc/nginx/conf.d/default.conf

COPY index.html /var/www/html/

WORKDIR /usr/share/nginx/html/

EXPOSE 80

STOPSIGNAL SIGTERM

CMD ["nginx", "-g", "daemon off;"]
