FROM php:8.2-fpm

RUN apt-get update && apt-get install -y \
    apt-utils \
    git \
    htop \
    libicu-dev \
    libpng-dev \
    libzip-dev \
    zlib1g-dev \
    libwebp-dev \
    libavif-dev \
    libfcgi-dev \
    nano \
    unzip \
    wget \
    screen \
    imagemagick \
    libmagickwand-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev

# PHP extensions
RUN docker-php-ext-configure gd --with-webp --with-avif --with-freetype --with-jpeg --enable-gd && \
    docker-php-ext-install -j$(nproc) pdo_mysql mysqli intl zip gd exif sockets && \
    pecl install imagick && \
    docker-php-ext-enable imagick

# Install FPM healthcheck
RUN wget -O /usr/local/bin/php-fpm-healthcheck \
    https://raw.githubusercontent.com/renatomefi/php-fpm-healthcheck/master/php-fpm-healthcheck \
    && chmod +x /usr/local/bin/php-fpm-healthcheck
ENV FCGI_CONNECT=/sock/php-fpm.sock

# composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

RUN mkdir /sock
COPY zz-docker.conf /usr/local/etc/php-fpm.d/zz-docker.conf


