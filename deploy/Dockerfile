# Build context is the repository root. The Laravel app lives in backend/.
# Render settings: Dockerfile Path = deploy/Dockerfile, Docker Build Context Directory = .

FROM composer:2 AS vendor
WORKDIR /app
COPY backend/composer.json backend/composer.lock ./
RUN composer install --no-dev --no-scripts --no-autoloader --prefer-dist --ignore-platform-reqs
COPY backend/ ./
RUN composer dump-autoload --optimize --no-dev --ignore-platform-reqs

FROM php:8.4-fpm-alpine
RUN apk add --no-cache nginx supervisor postgresql-dev libzip-dev \
    && docker-php-ext-install pdo_pgsql opcache pcntl
WORKDIR /var/www/html
COPY --from=vendor /app ./
COPY deploy/nginx.conf /etc/nginx/http.d/default.conf
COPY deploy/supervisord.conf /etc/supervisord.conf
COPY deploy/entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh \
    && mkdir -p storage/framework/cache storage/framework/sessions storage/framework/views storage/logs bootstrap/cache \
    && chown -R www-data:www-data storage bootstrap/cache
EXPOSE 10000
CMD ["/entrypoint.sh"]
