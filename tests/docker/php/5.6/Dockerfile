FROM php:5.6-cli

RUN apt update -qq \
    && apt install -y ca-certificates \
    && sed -i '/^mozilla\/DST_Root_CA_X3.crt$/ s/^/!/' /etc/ca-certificates.conf \
    && update-ca-certificates \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
    && apt install unzip \
    && apt-get install -y curl \
    && apt-get install -y zlib1g-dev \
    && docker-php-ext-install zip \
    && apt-get install -y libicu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-install pcntl \
    && pecl install igbinary-2.0.8 \
    && docker-php-ext-enable igbinary \
    && docker-php-ext-install bcmath \
    && docker-php-ext-install pdo_mysql \
    && apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql \
    && apt-get install -y libgearman-dev \
    && pecl install gearman-1.1.2 \
    && docker-php-ext-enable gearman \
    && rm -rf /var/lib/apt/lists/* \
    && curl -L -o /tmp/composer-setup.php https://getcomposer.org/installer \
    && php /tmp/composer-setup.php --install-dir=/usr/local/bin --filename=composer \
    && rm /tmp/composer-setup.php \
    && curl -L -o /usr/local/bin/php-cs-fixer https://cs.symfony.com/download/php-cs-fixer-v2.phar \
    && chmod a+x /usr/local/bin/php-cs-fixer

COPY . /code
WORKDIR /code

ENTRYPOINT ["tests/docker/php/entrypoint.sh"]
CMD ["sleep", "infinity"]
