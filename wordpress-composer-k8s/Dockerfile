FROM composer:latest AS composer

WORKDIR /app

COPY composer.json ./

# Forceer HTTPS voor GitHub
RUN git config --global url."https://github.com/".insteadOf "git@github.com:"

RUN composer install --no-dev --no-scripts --no-autoloader

FROM wordpress:php8.2-fpm

# Install required PHP extensions
RUN apt-get update && apt-get install -y \
    libzip-dev \
    zip \
    unzip \
    && docker-php-ext-install zip

# Create WordPress directory structure
RUN mkdir -p /var/www/html/web/wp

# Copy WordPress files from Composer
COPY --from=composer /app/vendor/johnpbloch/wordpress/ /var/www/html/web/wp/

# Set permissions
RUN chown -R www-data:www-data /var/www/html/web/wp && \
    chmod -R 755 /var/www/html/web/wp

# Copy wp-config.php
COPY wp-config.php /var/www/html/web/wp/

# Set working directory
WORKDIR /var/www/html/web/wp

# Create themes directory and set permissions
RUN mkdir -p /var/www/html/web/wp/wp-content/themes && \
    chown -R www-data:www-data /var/www/html/web/wp/wp-content/themes && \
    chmod -R 755 /var/www/html/web/wp/wp-content/themes

# Expose port 9000
EXPOSE 9000

CMD ["php-fpm"] 