FROM php:7.4.2-fpm

RUN apt-get update && apt-get install -y \
        curl \
        wget \
        git \
        unzip \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng-dev \
        libzip-dev \
        libonig-dev \
        gnupg \
    && pecl install mcrypt-1.0.3 \
    && docker-php-ext-install -j$(nproc) iconv mbstring zip opcache \
    && docker-php-ext-enable mcrypt opcache \
    && docker-php-ext-configure gd

RUN echo "$(curl -sS https://composer.github.io/installer.sig) -" > composer-setup.php.sig \
    && curl -sS https://getcomposer.org/installer | tee composer-setup.php | sha384sum -c composer-setup.php.sig \
    && php composer-setup.php && rm composer-setup.php* \
    && chmod +x composer.phar && mv composer.phar /usr/bin/composer

COPY local.ini /etc/php7/conf.d/
COPY local.ini /etc/php7/cli/conf.d/
COPY opcache.ini /usr/local/etc/php7/conf.d/

RUN usermod -u 1000 www-data

CMD ["php-fpm"]
WORKDIR /var/www/chatbot
EXPOSE 9000