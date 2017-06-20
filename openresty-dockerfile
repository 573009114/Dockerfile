FROM alpine:3.5
MAINTAINER haowen
ENV NGX_PREFIX=/opt/openresty/nginx/sbin/

WORKDIR /tmp
RUN apk add --no-cache --virtual .build-deps \
		gcc \
		libc-dev \
		make \
		openssl-dev \
		pcre-dev \
		zlib-dev \
		linux-headers \
		curl \
		gnupg \
		libxslt-dev \
		gd-dev \
		perl-dev \
		geoip-dev \
	&& wget http://10.15.201.252/software/xcar/ngx_cache_purge-2.3.tar.gz \
	&& wget http://10.15.201.252/software/xcar/ngx_openresty-1.7.10.2.tar.gz \
	&& wget http://10.15.201.252/software/xcar/nginx-http-concat_20161124.tgz \
	&& tar zxf ngx_cache_purge-2.3.tar.gz \
	&& tar zxf ngx_openresty-1.7.10.2.tar.gz \
	&& tar zxf nginx-http-concat_20161124.tgz \
	&& cd ngx_openresty-1.7.10.2 \
	&& ./configure --prefix=/opt/openresty --with-http_stub_status_module --with-http_sub_module --with-http_auth_request_module --add-module=../nginx-http-concat --add-module=../ngx_cache_purge-2.3 \
	&& make && make install \
	&& rm -fr nginx* ngx* \
	&& apk add --no-cache --virtual .gettext gettext \
	&& runDeps="$( \
		scanelf --needed --nobanner /opt/openresty/nginx/sbin/nginx /usr/lib/*.so  \
			| awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
			| sort -u \
			| xargs -r apk info --installed \
			| sort -u \
	)" \
	&& apk add --no-cache --virtual .nginx-rundeps $runDeps \
	&& apk del .build-deps \
	&& apk del .gettext \
	&& rm -fr /tmp/nginx-http-concat* \
	&& rm -fr /tmp/ngx_cache_purge* \
	&& rm -fr /tmp/ngx_openresty* 

EXPOSE 80 443
WORKDIR $NGX_PREFIX
CMD ["./nginx","-g","daemon off;"]
