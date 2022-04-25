FROM php:8.1.4-fpm

RUN apt-get update && apt-get install -y zlib1g-dev libicu-dev g++ libzip-dev libpng-dev apt-utils unzip wkhtmltopdf dnsutils
RUN apt-get update && apt install -y curl

RUN apt-get update && apt-get install -y \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

RUN apt-get update && apt-get install mariadb-client -y

RUN apt-get update \
    && curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.34.0/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install 14.15.5
RUN . "$NVM_DIR/nvm.sh" && nvm use v14.15.5
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v14.15.5
ENV PATH="/root/.nvm/versions/node/v14.15.5/bin/:${PATH}"

RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Extension driver
RUN docker-php-ext-configure intl \
    && docker-php-ext-install intl exif \
    && docker-php-ext-install pdo_mysql mysqli gd zip
RUN pecl install redis && docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV HOST 0.0.0.0

CMD ["php-fpm"]