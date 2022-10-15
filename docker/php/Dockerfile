FROM php:8.1-apache-bullseye

RUN apt-get update \
 && curl -fsSL https://deb.nodesource.com/setup_18.x | bash - \
 && curl -fsSL https://salsa.debian.org/postgresql/postgresql-common/raw/master/pgdg/apt.postgresql.org.sh | PGVERSION=15 bash - \
 && apt-get install -y --no-install-recommends \
    bash-completion \
    git \
    jq \
    libbz2-dev \
    libfreetype6 \
    libfreetype6-dev \
    libjpeg62-turbo \
    libjpeg62-turbo-dev \
    libmagickcore-6.q16-6 \
    libmagickcore-6.q16-dev \
    libmagickwand-6.q16-6 \
    libmagickwand-6.q16-dev \
    libpng-dev \
    libpng-tools \
    libpng16-16 \
    libpq-dev \
    libssl-dev \
    libxslt1-dev \
    libxslt1.1 \
    libzip-dev \
    libzip4 \
    netcat \
    nodejs \
    postgresql-client-15 \
    ssh \
    tree \
    unzip \
    vim \
 && docker-php-ext-configure gd --with-freetype --with-jpeg \
 && docker-php-ext-install -j$(nproc) \
    bcmath \
    bz2 \
    calendar \
    exif \
    gd \
    gettext \
    intl \
    opcache \
    pcntl \
    pdo_pgsql \
    pgsql \
    sockets \
    xsl \
    zip \
 && pecl install apcu \
 && pecl install imagick \
 && pecl install pcov \
 && pecl clear-cache \
 && docker-php-ext-enable apcu imagick pcov \
 && apt-get purge -y \
    libbz2-dev \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmagickcore-6.q16-dev \
    libmagickwand-6.q16-dev \
    libpng-dev \
    libpq-dev \
    libssl-dev \
    libxslt1-dev \
    libzip-dev \
 && apt-get -y autoremove \
 && apt-get clean \
 && curl -fsSL https://getcomposer.org/download/latest-2.x/composer.phar -o /usr/local/bin/composer \
 && chmod +x /usr/local/bin/composer \
 && curl https://raw.githubusercontent.com/git/git/master/contrib/completion/git-completion.bash -o /etc/bash_completion.d/git-completion.bash -s \
 && echo . /etc/bash_completion >> /etc/bash.bashrc \
 && rm -rf /var/www/* \
 && chmod g+s /var/www \
 && chown -R www-data:www-data /var/www \
 && chsh -s /bin/bash www-data \
 && mkdir /home/www-data \
 && usermod -d /home/www-data www-data \
 && cp -r /etc/skel/. /home/www-data \
 && chown -R www-data:www-data /home/www-data

ENV PATH $PATH:vendor/bin:node_modules/.bin
WORKDIR  /var/www
USER www-data
ENTRYPOINT []
CMD ["/usr/sbin/apache2", "-T", "-D", "FOREGROUND"]
