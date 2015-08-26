FROM ubuntu:14.04
MAINTAINER Derek Bourgeois <derek@ibourgeois.com>

RUN apt-get update
RUN apt-get upgrade -y

# Install for: 
#   - ibourgeois/mysql
RUN apt-get install -y perl --no-install-recommends 

# Install for:
#   - ibourgeois/nginx
#   - ibourgeois/php
RUN apt-get install -y ca-certificates --no-install-recommends

# Install for:
#   - ibourgeois/php
RUN apt-get install -y curl libpcre3 librecode0 libsqlite3-0 libxml2 \
    autoconf file g++ gcc libc-dev make pkg-config re2c tar --no-install-recommends

RUN rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
