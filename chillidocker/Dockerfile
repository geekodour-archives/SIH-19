FROM ubuntu:15.04
MAINTAINER Sylvain Desbureaux <sylvain@desbureaux.fr>

RUN apt-get update && apt-get install -y \
  git \
  build-essential \
  libtool \
  autoconf \
  automake \
  gengetopt \
  devscripts \
  debhelper \
  libssl-dev \
  iptables \
  haserl \
  net-tools

# grep git version of coova-chilli
RUN git clone --depth 2 https://github.com/coova/coova-chilli.git /src/coova-chilli

WORKDIR /src/coova-chilli

# create package
RUN debuild -us -uc -b

# install package
RUN dpkg -i ../coova-chilli_*.deb

# clean 
RUN apt-get purge -y git build-essential libtool autoconf automake gengetopt devscripts debhelper && \
    apt-get autoremove -y && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# put right config
COPY chilli.conf /etc/chilli.conf
COPY config /etc/chilli/config
COPY hs.conf /etc/chilli/hs.conf
COPY local.conf /etc/chilli/local.conf
COPY main.conf /etc/chilli/main.conf

EXPOSE 3990 4990

USER chilli

COPY chilli.conf /etc/chilli.conf

VOLUME /config

ENTRYPOINT ["/usr/sbin/chilli", "--fg"]
CMD ["--conf", "/config/chilli.conf"]
