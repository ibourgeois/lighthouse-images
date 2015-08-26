FROM ibourgeois/base
MAINTAINER Derek Bourgeois <derek@ibourgeois.com>

RUN apt-get update
RUN apt-get -y upgrade

RUN echo "LC_ALL=en_US.UTF-8" >> /etc/default/locale
RUN locale-gen en_US.UTF-8

RUN apt-get install -y software-properties-common curl wget nano

RUN apt-add-repository ppa:nginx/stable -y
RUN apt-add-repository ppa:rwky/redis -y
RUN apt-add-repository ppa:ondrej/php5-5.6 -y

RUN curl -s https://packagecloud.io/gpg.key | apt-key add -
RUN echo "deb http://packages.blackfire.io/debian any main" | tee /etc/apt/sources.list.d/blackfire.list

RUN curl --silent --location https://deb.nodesource.com/setup_0.12 | bash -

RUN apt-get update

RUN apt-get install -y build-essential dos2unix gcc git libmcrypt4 libpcre3-dev \
    make python2.7-dev python-pip re2c supervisor unattended-upgrades whois vim

RUN ln -sf /usr/share/zoneinfo/UTC /etc/localtime

RUN apt-get install -y php5-cli php5-dev php-pear \
    php5-mysqlnd php5-pgsql php5-sqlite \
    php5-apcu php5-json php5-curl php5-gd \
    php5-gmp php5-imap php5-mcrypt php5-xdebug \
    php5-memcached

RUN ln -s /etc/php5/conf.d/mcrypt.ini /etc/php5/mods-available
RUN php5enmod mcrypt

RUN pecl install mailparse
RUN echo "extension=mailparse.so" > /etc/php5/mods-available/mailparse.ini
RUN ln -s /etc/php5/mods-available/mailparse.ini /etc/php5/cli/conf.d/20-mailparse.ini

RUN curl -sS https://getcomposer.org/installer | php
RUN mv composer.phar /usr/local/bin/composer

RUN printf "\nPATH=\"/home/lighthouse/.composer/vendor/bin:\$PATH\"\n" | tee -a /home/lighthouse/.profile

RUN /usr/local/bin/composer global require "laravel/envoy=~1.0"

RUN sed -i "s/error_reporting = .*/error_reporting = E_ALL/" /etc/php5/cli/php.ini
RUN sed -i "s/display_errors = .*/display_errors = On/" /etc/php5/cli/php.ini
RUN sed -i "s/memory_limit = .*/memory_limit = 512M/" /etc/php5/cli/php.ini
RUN sed -i "s/;date.timezone.*/date.timezone = UTC/" /etc/php5/cli/php.ini

RUN apt-get install -y nginx php5-fpm

RUN rm /etc/nginx/sites-enabled/default
RUN rm /etc/nginx/sites-available/default
RUN service nginx restart

RUN wget -O - http://dl.hhvm.com/conf/hhvm.gpg.key | apt-key add -
RUN echo deb http://dl.hhvm.com/ubuntu trusty main | tee /etc/apt/sources.list.d/hhvm.list
RUN apt-get update
RUN apt-get install -y hhvm

RUN service hhvm stop
RUN sed -i 's/#RUN_AS_USER="www-data"/RUN_AS_USER="lighthouse/' /etc/default/hhvm
RUN service hhvm start

RUN update-rc.d hhvm defaults

RUN ln -s /etc/php5/mods-available/mailparse.ini /etc/php5/fpm/conf.d/20-mailparse.ini

RUN sed -i "s/error_reporting = .*/error_reporting = E_ALL/" /etc/php5/fpm/php.ini
RUN sed -i "s/display_errors = .*/display_errors = On/" /etc/php5/fpm/php.ini
RUN sed -i "s/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/" /etc/php5/fpm/php.ini
RUN sed -i "s/memory_limit = .*/memory_limit = 512M/" /etc/php5/fpm/php.ini
RUN sed -i "s/upload_max_filesize = .*/upload_max_filesize = 100M/" /etc/php5/fpm/php.ini
RUN sed -i "s/post_max_size = .*/post_max_size = 100M/" /etc/php5/fpm/php.ini
RUN sed -i "s/;date.timezone.*/date.timezone = UTC/" /etc/php5/fpm/php.ini

RUN echo "xdebug.remote_enable = 1" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_connect_back = 1" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.remote_port = 9000" >> /etc/php5/fpm/conf.d/20-xdebug.ini
RUN echo "xdebug.max_nesting_level = 512" >> /etc/php5/fpm/conf.d/20-xdebug.ini

RUN echo " \n\
fastcgi_param	QUERY_STRING		\$query_string; \n\
fastcgi_param	REQUEST_METHOD		\$request_method; \n\
fastcgi_param	CONTENT_TYPE		\$content_type; \n\
fastcgi_param	CONTENT_LENGTH		\$content_length; \n\
fastcgi_param	SCRIPT_FILENAME		\$request_filename; \n\
fastcgi_param	SCRIPT_NAME		    \$fastcgi_script_name; \n\
fastcgi_param	REQUEST_URI		    \$request_uri; \n\
fastcgi_param	DOCUMENT_URI		\$document_uri; \n\
fastcgi_param	DOCUMENT_ROOT		\$document_root; \n\
fastcgi_param	SERVER_PROTOCOL		\$server_protocol; \n\
fastcgi_param	GATEWAY_INTERFACE	CGI/1.1; \n\
fastcgi_param	SERVER_SOFTWARE		nginx/\$nginx_version; \n\
fastcgi_param	REMOTE_ADDR		    \$remote_addr; \n\
fastcgi_param	REMOTE_PORT		    \$remote_port; \n\
fastcgi_param	SERVER_ADDR		    \$server_addr; \n\
fastcgi_param	SERVER_PORT		    \$server_port; \n\
fastcgi_param	SERVER_NAME		    \$server_name; \n\
fastcgi_param	HTTPS			    \$https if_not_empty; \n\
fastcgi_param	REDIRECT_STATUS		200; \n\
" >> /etc/nginx/fastcgi_params

RUN sed -i "s/user www-data;/user lighthouse;/" /etc/nginx/nginx.conf
RUN sed -i "s/# server_names_hash_bucket_size.*/server_names_hash_bucket_size 64;/" /etc/nginx/nginx.conf

RUN sed -i "s/user = www-data/user = lighthouse/" /etc/php5/fpm/pool.d/www.conf
RUN sed -i "s/group = www-data/group = lighthouse/" /etc/php5/fpm/pool.d/www.conf

RUN sed -i "s/listen\.owner.*/listen.owner = lighthouse/" /etc/php5/fpm/pool.d/www.conf
RUN sed -i "s/listen\.group.*/listen.group = lighthouse/" /etc/php5/fpm/pool.d/www.conf
RUN sed -i "s/;listen\.mode.*/listen.mode = 0666/" /etc/php5/fpm/pool.d/www.conf

RUN service nginx restart
RUN service php5-fpm restart

RUN usermod -a -G www-data lighthouse
RUN id lighthouse
RUN groups lighthouse

RUN apt-get install -y nodejs
RUN /usr/bin/npm install -g grunt-cli
RUN /usr/bin/npm install -g gulp
RUN /usr/bin/npm install -g bower

RUN apt-get install -y sqlite3 libsqlite3-dev

RUN debconf-set-selections <<< "mysql-server mysql-server/root_password password secret"
RUN debconf-set-selections <<< "mysql-server mysql-server/root_password_again password secret"
RUN apt-get install -y mysql-server-5.6

RUN sed -i '/^bind-address/s/bind-address.*=.*/bind-address = 0.0.0.0/' /etc/mysql/my.cnf
RUN mysql --user="root" --password="secret" -e "GRANT ALL ON *.* TO root@'0.0.0.0' IDENTIFIED BY 'secret' WITH GRANT OPTION;"
RUN service mysql restart

RUN mysql --user="root" --password="secret" -e "CREATE USER 'lighthouse'@'0.0.0.0' IDENTIFIED BY 'secret';"
RUN mysql --user="root" --password="secret" -e "GRANT ALL ON *.* TO 'lighthouse'@'0.0.0.0' IDENTIFIED BY 'secret' WITH GRANT OPTION;"
RUN mysql --user="root" --password="secret" -e "GRANT ALL ON *.* TO 'lighthouse'@'%' IDENTIFIED BY 'secret' WITH GRANT OPTION;"
RUN mysql --user="root" --password="secret" -e "FLUSH PRIVILEGES;"
RUN mysql --user="root" --password="secret" -e "CREATE DATABASE lighthouse;"
RUN service mysql restart

RUN mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql --user=root --password=secret mysql

RUN apt-get install -y blackfire-agent blackfire-php

RUN apt-get install -y redis-server memcached beanstalkd

RUN sed -i "s/#START=yes/START=yes/" /etc/default/beanstalkd
RUN /etc/init.d/beanstalkd start

RUN /bin/dd if=/dev/zero of=/var/swap.1 bs=1M count=1024
RUN /sbin/mkswap /var/swap.1
RUN /sbin/swapon /var/swap.1

RUN dd if=/dev/zero of=/EMPTY bs=1M
RUN rm -f /EMPTY
RUN sync

VOLUME /usr/share/nginx/html
VOLUME /etc/nginx

EXPOSE 80 443
CMD ["/bin/bash"]
