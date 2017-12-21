FROM debian:jessie-slim

RUN apt-get update &&\
    apt-get -y install gcc gfortran python-dev openssl bash make g++ curl rsync cmake perl libboost-dev wget bzip2

RUN mkdir /code

WORKDIR /code

# LHAPDF
RUN wget http://www.hepforge.org/archive/lhapdf/LHAPDF-6.1.6.tar.gz &&\
tar xfz LHAPDF-6.1.6.tar.gz &&\
cd LHAPDF-6.1.6 &&\
./configure --prefix=/usr &&\
make &&\
make install &&\
cd /code && rm -rf LHAPDF-6.1.6

# Fastjet
RUN  curl -O fastjet.fr/repo/fastjet-3.3.0.tar.gz &&\
  tar xfz fastjet-3.3.0.tar.gz &&\
  cd fastjet-3.3.0/ &&\
  ./configure --prefix=/usr &&\
  make &&\
  make install &&\
  cd /code && rm -rf fastjet-3.3.0/

RUN cd /code && \
    wget http://www.hepforge.org/archive/sherpa/SHERPA-MC-2.2.4.tar.gz && \
    tar xfz SHERPA-MC-2.2.4.tar.gz && cd SHERPA-MC-2.2.4 && \
    ./configure --with-sqlite3=install --enable-lhapdf=/usr --enable-fastjet=/usr && \
    make && make install && cd .. && rm -rf SHERPA-MC-2.2.4.tar.gz
