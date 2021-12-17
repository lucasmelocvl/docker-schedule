FROM php:8.0-cli AS builder

WORKDIR /var/www/

RUN mkdir dletra-schedule

COPY src /dletra-schedule

RUN chown -R root:root /var/www/dletra-schedule

RUN apt-get update && \
    apt-get install libzip-dev -y && \
    docker-php-ext-install zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');"

#RUN php composer.phar create-project laravel/laravel dletra-schedule

#ENTRYPOINT [ "php", "dletra-schedule/artisan", "serve" ]
#CMD ["--host=0.0.0.0"]

FROM php:8.0-fpm-alpine
WORKDIR /var/www
RUN rm -rf /var/www/html
COPY --from=builder /var/www/dletra-schedule .
RUN ln -s dletra-schedule/public html
RUN chown -R www-data:www-data /var/www
EXPOSE 9000
CMD ["php-fpm"]