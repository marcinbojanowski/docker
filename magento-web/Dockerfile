FROM php:7.4-fpm

# Add s6-overlay
ENV S6_OVERLAY_VERSION=v1.19.1.1 \
    NGINX_VERSION=nginx-1.21.4 \
    S6_FIX_ATTRS_HIDDEN=1 \
    COMPOSER_ALLOW_SUPERUSER=1 \
    TZ="America/Chicago"

RUN groupadd -g 1000 magento \
        && adduser -u 1000 --disabled-password --gid 1000 magento

RUN apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
	software-properties-common \
	&& apt-get update \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -y \
	libfreetype6-dev \
	libicu-dev \
    libssl-dev \
    libzip-dev \
    zlib1g-dev libpng-dev \
    libjpeg-dev \
	libjpeg62-turbo-dev \
	libmcrypt-dev \
	libedit-dev \
	libedit2 \
	libxslt1-dev \
	apt-utils \
	gnupg \
	redis-tools \
	mariadb-client \
	git \
	vim \
	wget \
	curl \
	lynx \
	psmisc \
	unzip \
	tar \
	cron \
	bash-completion \
	nginx \
    sudo \
    netcat \
    bc \
	&& apt-get clean

# Install Magento Dependencies

RUN docker-php-ext-configure \
  	gd --with-jpeg --with-freetype; \
  	docker-php-ext-install \
  	opcache \
  	gd \
  	bcmath \
  	intl \
  	pdo_mysql \
  	soap \
  	xsl \
  	zip \
    sockets

# Install oAuth

RUN apt-get update \
  	&& apt-get install -y \
  	libpcre3 \
  	libpcre3-dev \
  	# php-pear \
  	&& pecl install oauth \
  	&& echo "extension=oauth.so" > /usr/local/etc/php/conf.d/docker-php-ext-oauth.ini

    # opcache config
RUN docker-php-ext-configure opcache --enable-opcache \
    && docker-php-ext-install opcache \
    && cd /usr/local/etc/php/conf.d \
    && echo "opcache.enable=1" >> docker-php-ext-opcache.ini \
    && echo "opcache.enable_cli=1" >> docker-php-ext-opcache.ini \
    && echo "opcache.memory_consumption=512" >> docker-php-ext-opcache.ini \
    && echo "opcache.interned_strings_buffer=12" >> docker-php-ext-opcache.ini \
    && echo "opcache.fast_shutdown=1" >> docker-php-ext-opcache.ini \
    && echo "opcache.revalidate_freq=0" >> docker-php-ext-opcache.ini \
    && echo "opcache.max_accelerated_files=65406" >> docker-php-ext-opcache.ini

# Install oAuth

RUN apt-get update \
  	&& apt-get install -y \
  	libpcre3 \
  	libpcre3-dev \
  	&& echo "extension=oauth.so" > /usr/local/etc/php/conf.d/docker-php-ext-oauth.ini


RUN	curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin/ --filename=composer

ENV PATH="/var/www/.composer/vendor/bin/:${PATH}"

# install s6-overlay
RUN curl -sSL https://github.com/just-containers/s6-overlay/releases/download/${S6_OVERLAY_VERSION}/s6-overlay-amd64.tar.gz \
           | tar xfz - -C /

# root filesystem
COPY rootfs /

# Install Magerun 2
RUN wget --quiet https://files.magerun.net/n98-magerun2.phar \
	&& chmod +x ./n98-magerun2.phar \
	&& mv ./n98-magerun2.phar /usr/local/bin/
run mkdir -p /var/run/php-fpm
RUN sed -i "s/listen = 127.0.0.1:9000/listen = '\/var\/run\/php-fpm\/php-fpm.sock'/g" /usr/local/etc/php-fpm.d/www.conf

STOPSIGNAL SIGQUIT
EXPOSE 80
WORKDIR /var/www/magento
ENTRYPOINT [ "/init" ]

