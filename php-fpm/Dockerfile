FROM ibourgeois/base
MAINTAINER Derek Bourgeois <derek@ibourgeois.com>

RUN apt-get update
RUN apt-get install -y php-fpm --no-install-recommends
RUN rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html
COPY php-fpm.conf /usr/local/etc/

EXPOSE 9000
CMD ["php-fpm"]
