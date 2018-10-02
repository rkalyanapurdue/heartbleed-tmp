FROM ubuntu:trusty

RUN apt-get update
RUN apt-get -y install python python-pip

RUN mkdir -p /srv/www/app

ADD . /srv/www/app

WORKDIR /srv/www/app

RUN tar -xvf openssl_1.0.1f.orig.tar

WORKDIR openssl-1.0.1f

RUN ./config

RUN make

RUN make install_sw

RUN apt-get -y install ssl-cert apache2

RUN a2enmod ssl
RUN a2ensite default-ssl
RUN /etc/init.d/apache2 restart

EXPOSE 80
EXPOSE 443

ENTRYPOINT ["/usr/sbin/apachectl","-DFOREGROUND"]
