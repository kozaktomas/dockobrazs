FROM php:8.2-cli-alpine

# Runtime packages (tools + libs for extensions)
RUN set -eux; \
    apk add --no-cache \
      bash \
      git \
      htop \
      nano \
      screen \
      unzip \
      wget \
      imagemagick \
      fcgi \
      libavif \
      libpng \
      libjpeg-turbo \
      freetype \
      libwebp \
      libzip \
      zlib \
      icu-libs \
      libstdc++

# Build extensions + imagick, then remove build deps for a smaller image
RUN set -eux; \
    apk add --no-cache --virtual .build-deps \
      $PHPIZE_DEPS \
      build-base \
      linux-headers \
      pkgconf \
      autoconf \
      imagemagick-dev \
      libavif-dev \
      libpng-dev \
      libjpeg-turbo-dev \
      freetype-dev \
      libwebp-dev \
      libzip-dev \
      zlib-dev \
      icu-dev \
    && docker-php-ext-configure gd \
         --with-freetype \
         --with-jpeg \
         --with-webp \
         --with-avif \
    && docker-php-ext-install -j"$(nproc)" \
         pdo_mysql \
         mysqli \
         intl \
         zip \
         gd \
         exif \
         sockets \
         pcntl \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apk del .build-deps \
    && rm -rf /tmp/pear ~/.pearrc

# Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php composer-setup.php --install-dir=/bin --filename=composer \
    && php -r "unlink('composer-setup.php');"

