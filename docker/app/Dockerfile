FROM ubuntu:18.04

LABEL author="Yazeed Obaid"
LABEL version=1.0.0
LABEL description="Docker image for a PHP Laravel application"

ARG PHP_VERSION=7.4

ENV DEBIAN_FRONTEND=noninteractive

RUN set -x && apt-get update && apt-get install -y gnupg

RUN echo "deb http://ppa.launchpad.net/ondrej/php/ubuntu bionic main" > /etc/apt/sources.list.d/ppa_ondrej_php.list \
    && echo "deb http://ppa.launchpad.net/nginx/development/ubuntu bionic main" > /etc/apt/sources.list.d/ppa_nginx_mainline.list \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E5267A6C \
    && apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys C300EE8C \
    && apt-get update \
    && apt-get install -y curl zip unzip git supervisor sqlite3 \
    && apt-get install -y nginx php${PHP_VERSION}-fpm php${PHP_VERSION}-cli \
    php${PHP_VERSION}-pgsql php${PHP_VERSION}-sqlite3 php${PHP_VERSION}-gd \
    php${PHP_VERSION}-curl php${PHP_VERSION}-memcached \
    php${PHP_VERSION}-imap php${PHP_VERSION}-mysql php${PHP_VERSION}-mbstring \
    php${PHP_VERSION}-xml php${PHP_VERSION}-zip php${PHP_VERSION}-bcmath php${PHP_VERSION}-soap \
    php${PHP_VERSION}-intl php${PHP_VERSION}-readline php${PHP_VERSION}-xdebug \
    php${PHP_VERSION}-msgpack php${PHP_VERSION}-igbinary php${PHP_VERSION}-ldap \
    php-redis \
    # installing composer
    && php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'c31c1e292ad7be5f49291169c0ac8f683499edddcfd4e42232982d0fd193004208a58ff6f353fde0012d35fdd72bc394') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php -- --install-dir=/usr/bin/ --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    # clean ups and ensuring that php-fpm has the run/php directory to put its file on when running
    && mkdir /run/php \
    && apt-get remove -y --purge software-properties-common \
    && apt-get -y autoremove \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    # configuring nginx, pushing nginx to background when run
    && echo "daemon off;" >> /etc/nginx/nginx.conf \
    # simlinking log files of nginx to standard output and standard error to enable Docker to read and display for us
    && ln -sf /dev/stdout /var/log/nginx/access.log \
    && ln -sf /dev/stderr /var/log/nginx/error.log

# ports to expose for other containers
EXPOSE 80

# working directory is the project root
WORKDIR /var/www/html

# copying nginx's site configuration for the application
COPY  default /etc/nginx/sites-available/default

# copying modified php-fpm file to php-fpm in container, mainly for simlinking php-fpm logs
COPY  php-fpm.conf /etc/php/${PHP_VERSION}/fpm/php-fpm.conf

# copying supervisor configuration file to container
COPY  supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# copying the entrypoint script for the container.
COPY  docker-entrypoint /usr/bin/docker-entrypoint

# make script executable
RUN chmod +x /usr/bin/docker-entrypoint

# lunching supervisor to run nginx, php-fpm and Laravel queue worker if any
ENTRYPOINT ["docker-entrypoint"]
