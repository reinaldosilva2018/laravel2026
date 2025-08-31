FROM php:8.3-fpm-alpine

# Dependências do sistema
RUN apk add --no-cache \
    git curl unzip libpq-dev bash icu-dev oniguruma-dev zlib-dev libzip-dev \
    freetype-dev libjpeg-turbo-dev libpng-dev

# Extensões PHP necessárias para Laravel + Postgres
RUN docker-php-ext-configure gd \
  --with-freetype=/usr/include/ \
  --with-jpeg=/usr/include/
RUN docker-php-ext-install -j$(nproc) \
    gd \
    intl \
    mbstring \
    pdo \
    pdo_pgsql \
    zip \
    opcache

# Opcional: aumentar performance do FPM/opcache em dev
RUN { \
  echo 'opcache.enable=1'; \
  echo 'opcache.validate_timestamps=1'; \
  echo 'opcache.memory_consumption=128'; \
  echo 'opcache.max_accelerated_files=8000'; \
} > /usr/local/etc/php/conf.d/opcache.ini

# Instalar Composer (oficial)
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer

# Usuário não-root (opcional)
RUN addgroup -g 1000 app && adduser -D -G app -u 1000 app
USER app

WORKDIR /var/www/html
