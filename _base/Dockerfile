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
RUN apt-get install -y curl --no-install-recommends 
RUN apt-get install -y libpcre3 --no-install-recommends 
RUN apt-get install -y librecode0 --no-install-recommends 
RUN apt-get install -y libsqlite3-0 --no-install-recommends 
RUN apt-get install -y libxml2 --no-install-recommends 
RUN apt-get install -y autoconf --no-install-recommends 
RUN apt-get install -y file --no-install-recommends 
RUN apt-get install -y g++ --no-install-recommends 
RUN apt-get install -y gcc --no-install-recommends 
RUN apt-get install -y libc-dev --no-install-recommends 
RUN apt-get install -y make --no-install-recommends 
RUN apt-get install -y pkg-config --no-install-recommends 
RUN apt-get install -y re2c --no-install-recommends

RUN rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
