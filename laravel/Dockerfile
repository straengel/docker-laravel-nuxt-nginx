FROM php:8-fpm-alpine3.14


COPY composer.lock composer.json /var/www/

RUN docker-php-ext-install pdo pdo_mysql
RUN curl -sS https://getcomposer.org/installer | php -- \
        --install-dir=/usr/local/bin --filename=composer

WORKDIR /app

COPY . .

RUN composer install

COPY . .

#CMD php artisan serve --host=0.0.0.0
#FROM php:8-fpm-alpine3.14
#
#COPY composer.lock composer.json /var/www/
#
#
#WORKDIR /var/www
#
#RUN apk update
#RUN apk add \
#    zip \
#    jpegoptim optipng pngquant gifsicle \
#    vim \
#    unzip \
#    git
#RUN docker-php-ext-install pdo pdo_mysql
#
#RUN curl -sS https://getcomposer.org/installer | php -- \
#        --install-dir=/usr/local/bin --filename=composer
#RUN composer install
#
#
#COPY . /var/www
#
#
#EXPOSE 9000
#CMD ["php-fpm"]
#
##CMD php artisan serve --host=0.0.0.0
