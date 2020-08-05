FROM nginx:1.18.0 AS builder

ENV NGINX_VERSION 1.18.0
ENV CONFARGS="--prefix=/etc/nginx \
			  --sbin-path=/usr/sbin/nginx \
			  --modules-path=/usr/lib/nginx/modules \
			  --conf-path=/etc/nginx/nginx.conf \
			  --error-log-path=/var/log/nginx/error.log \
			  --http-log-path=/var/log/nginx/access.log \
			  --pid-path=/var/run/nginx.pid \
			  --lock-path=/var/run/nginx.lock \
			  --http-client-body-temp-path=/var/cache/nginx/client_temp \
			  --http-proxy-temp-path=/var/cache/nginx/proxy_temp \
			  --http-fastcgi-temp-path=/var/cache/nginx/fastcgi_temp \
			  --http-uwsgi-temp-path=/var/cache/nginx/uwsgi_temp \
			  --http-scgi-temp-path=/var/cache/nginx/scgi_temp \
			  --user=nginx \
			  --group=nginx \
			  --with-compat \
			  --with-file-aio \
			  --with-threads \
			  --with-http_addition_module \
			  --with-http_auth_request_module \
			  --with-http_dav_module \
			  --with-http_flv_module \
			  --with-http_gunzip_module \
			  --with-http_gzip_static_module \
			  --with-http_mp4_module \
			  --with-http_random_index_module \
			  --with-http_realip_module \
			  --with-http_secure_link_module \
			  --with-http_slice_module \
			  --with-http_ssl_module \
			  --with-http_stub_status_module \
			  --with-http_sub_module \
			  --with-http_v2_module \
			  --with-mail \
			  --with-mail_ssl_module \
			  --with-stream \
			  --with-stream_realip_module \
			  --with-stream_ssl_module \
			  --with-stream_ssl_preread_module \
			  --add-dynamic-module=/ngx_brotli"
RUN apt-get update &&  apt-get install --no-install-recommends --no-install-suggests -y \
  gnupg1 \
  libssl-dev \
  ca-certificates  \
  gcc \
  libc-dev \
  make \
  openssl\
  curl \
  gnupg \
  wget \
  libpcre3 libpcre3-dev \
  libghc-zlib-dev \
  git

RUN mkdir ngx_brotli && \ 
	cd ngx_brotli && \
	git clone https://github.com/google/ngx_brotli.git .&& \
	git submodule update --init --recursive  && \
	wget "http://nginx.org/download/nginx-${NGINX_VERSION}.tar.gz" -O nginx.tar.gz && \
 	tar -zxC /usr/src -f nginx.tar.gz

WORKDIR /usr/src/nginx-$NGINX_VERSION
RUN ./configure --with-compat $CONFARGS && \
  	make && make install


FROM nginx:1.18.0
COPY --from=builder /usr/lib/nginx/modules/*.so /etc/nginx/modules/

RUN rm /etc/nginx/conf.d/default.conf
COPY nginx.conf /etc/nginx/nginx.conf
COPY default.conf /etc/nginx/conf.d/default.conf

EXPOSE 443

CMD ["nginx", "-g", "daemon off;"]
