FROM ubuntu:trusty

RUN apt-get update
#RUN apt-get -y install openssl ssl-cert python python-pip
RUN apt-get -y install python python-pip

RUN mkdir -p /srv/www/app

ADD . /srv/www/app

RUN pip install flask flask-login Flask-WTF

WORKDIR /srv/www/app

RUN tar -xvf openssl_1.0.1f.orig.tar

WORKDIR openssl-1.0.1f

RUN ./config

RUN make

RUN make install_sw

RUN apt-get -y install ssl-cert

WORKDIR /srv/www/app

CMD ["python","hello.py"]

