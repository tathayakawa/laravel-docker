FROM php:8.4-apache-bookworm

RUN apt-get update \
 && apt-get install -y --no-install-recommends gnupg \
 && curl -sSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor -o /usr/share/keyrings/postgres-archive-keyring.gpg \
 && echo "deb [signed-by=/usr/share/keyrings/postgres-archive-keyring.gpg] https://apt.postgresql.org/pub/repos/apt/" bookworm-pgdg main > /etc/apt/sources.list.d/postgres.list \
 && curl -fsSL https://deb.nodesource.com/gpgkey/nodesource-repo.gpg.key | gpg --dearmor -o /usr/share/keyrings/nodesource.gpg \
 && echo "deb [signed-by=/usr/share/keyrings/nodesource.gpg] https://deb.nodesource.com/node_24.x nodistro main" | tee /etc/apt/sources.list.d/nodesource.list \
 && curl -fsSL https://cli.github.com/packages/githubcli-archive-keyring.gpg > /etc/apt/keyrings/githubcli-archive-keyring.gpg \
 && echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/githubcli-archive-keyring.gpg] https://cli.github.com/packages stable main" > /etc/apt/sources.list.d/github-cli.list \
 && apt-get update \
 && apt-get install -y --no-install-recommends \
    bash-completion \
    default-mysql-client \
    git \
    gh \
    jq \
    less \
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
    netcat-openbsd \
    nodejs \
    postgresql-client-17 \
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
    pdo_mysql \
    pdo_pgsql \
    sockets \
    xsl \
    zip \
 && pecl install apcu \
 && pecl install xdebug \
 && pecl clear-cache \
 && curl -L --output -  https://pecl.php.net/get/imagick-3.7.0.tgz  | tar xz -C /tmp \
 && (cd /tmp/imagick-3.7.0 && phpize && ./configure && make install) \
 && rm -rf /tmp/imagick-3.7.0 \
 && docker-php-ext-enable apcu imagick xdebug \
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
 && chmod g+s /var/www \
 && chown -R www-data:www-data /var/www \
 && chsh -s /bin/bash www-data \
 && mkdir /home/www-data \
 && usermod -d /home/www-data www-data \
 && cp -r /etc/skel/. /home/www-data \
 && chown -R www-data:www-data /home/www-data \
 && su -s /bin/bash www-data -c "curl -fsSL https://claude.ai/install.sh | bash"

COPY ssh-config /etc/ssh/ssh_config.d/default.conf
RUN chmod 644 /etc/ssh/ssh_config.d/default.conf
RUN rm -rf /var/www/*

ENV PATH=$PATH:vendor/bin:node_modules/.bin
WORKDIR  /var/www
USER www-data
ENTRYPOINT []
CMD ["/usr/sbin/apache2", "-T", "-D", "FOREGROUND"]
