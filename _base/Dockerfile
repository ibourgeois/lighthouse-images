FROM ubuntu:14.04
MAINTAINER Derek Bourgeois <derek@ibourgeois.com>

RUN apt-get update
RUN apt-get upgrade -y

RUN apt-get install -y perl ca-certificates --no-install-recommends 

RUN rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
