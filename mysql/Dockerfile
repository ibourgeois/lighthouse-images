FROM ibourgeois/base
MAINTAINER Derek Bourgeois <derek@ibourgeois.com>

RUN groupadd -r mysql 
RUN useradd -r -g mysql mysql

RUN mkdir /docker-entrypoint-initdb.d

EXPOSE 3306
CMD ["mysqld"]
