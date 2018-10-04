FROM ubuntu:trusty

RUN apt-get update
RUN apt-get -y install python python-pip
RUN apt-get install -y wget

#get Ubuntu trusty version of OpenSSL & libssl from the Debian snapshots
RUN wget http://snapshot.debian.org/archive/debian/20140106T220635Z/pool/main/o/openssl/openssl_1.0.1f-1_amd64.deb -O /tmp/openssl_1.0.1f-1_amd64.deb
RUN wget http://snapshot.debian.org/archive/debian/20140106T220635Z/pool/main/o/openssl/libssl1.0.0_1.0.1f-1_amd64.deb -O /tmp/libssl1.0.0_1.0.1f-1_amd64.deb

#install these versions of OpenSSL
RUN dpkg -i /tmp/libssl1.0.0_1.0.1f-1_amd64.deb
RUN dpkg -i /tmp/openssl_1.0.1f-1_amd64.deb

#adds the snakeoil self signed certificate
RUN apt-get -y install ssl-cert

#Simple Flask based webapp
RUN mkdir -p /srv/www/app

ADD app /srv/www/app

RUN pip install Flask Flask-Login Flask-WTF

WORKDIR /srv/www/app

CMD ["python","hello.py"]
