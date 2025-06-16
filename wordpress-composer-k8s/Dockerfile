FROM composer:latest as composer

WORKDIR /app
COPY composer.json ./
RUN composer install --no-dev --no-scripts --no-autoloader

FROM wordpress:php8.2-fpm

# Install dependencies
RUN apt-get update && apt-get install -y \
    libzip-dev \
    unzip \
    && docker-php-ext-install zip

WORKDIR /var/www/html

# Copy WordPress files from composer build
COPY --from=composer /app/vendor/johnpbloch/wordpress/ /var/www/html/
COPY --from=composer /app/vendor/ /var/www/html/vendor/
COPY --from=composer /app/composer.json /var/www/html/composer.json
COPY --from=composer /app/composer.lock /var/www/html/composer.lock

# Set permissions
RUN chown -R www-data:www-data /var/www/html && \
    chmod -R 755 /var/www/html && \
    find /var/www/html -type d -exec chmod 755 {} \; && \
    find /var/www/html -type f -exec chmod 644 {} \;

EXPOSE 9000

CMD ["php-fpm"] 