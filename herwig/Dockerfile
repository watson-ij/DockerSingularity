FROM debian:jessie-slim

RUN apt-get update && apt-get --yes install gcc g++ gfortran python-dev libboost-all-dev make patch subversion zlib1g-dev wget

# FROM cern/cc7-base
# 
# RUN yum -y update && yum -y install gcc python-devel subversion zlib-devel gcc-gfortran gcc-c++ wget boost-devel make patch

RUN mkdir /code && cd /code

WORKDIR /code

RUN wget https://herwig.hepforge.org/herwig-bootstrap && chmod +x herwig-bootstrap

RUN ./herwig-bootstrap --with-boost=/usr herwig && rm -rf /code/herwig/src

ADD entry.sh /code/entry.sh
RUN chmod +x /code/entry.sh

RUN mkdir /work

RUN apt-get --yes install vim

ENTRYPOINT ["/code/entry.sh"]
