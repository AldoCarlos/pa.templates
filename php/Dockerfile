FROM php:7.2-cli

# Instal OS dependencies
RUN apt-get -y update
RUN apt-get install -y zip unzip git

WORKDIR /var/www/html

# Instal composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
 && php composer-setup.php

# Instal composer dependencies
COPY composer.lock /var/www/html
COPY composer.json /var/www/html
RUN php composer.phar config cache-dir
RUN php composer.phar install --no-interaction --no-scripts --no-autoloader
COPY . /var/www/html
RUN php composer.phar dump-autoload -d /var/www/html
RUN chown -R www-data /var/www/html

EXPOSE 80
CMD ["php", "-d variables_order=EGPCS", "-S", "0.0.0.0:80"]
