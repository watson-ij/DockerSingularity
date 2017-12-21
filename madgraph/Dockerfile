FROM debian:jessie-slim

RUN apt-get update &&\
    apt-get -y install gcc gfortran python-dev openssl bash make g++ curl rsync cmake perl libboost-dev wget

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

# Setup MG5_aMC
RUN wget https://launchpad.net/mg5amcnlo/2.0/2.6.x/+download/MG5_aMC_v2.6.0.tar.gz && \
    tar xfz MG5_aMC_v2.6.0.tar.gz && cd MG5_aMC_v2_6_0 &&\
    (echo "set lhapdf /usr/bin/lhapdf-config" | ./bin/mg5_aMC) &&\
    (echo "set fastjet /usr/bin/fastjet-config" | ./bin/mg5_aMC) &&\
    (echo "install pythia8" | ./bin/mg5_aMC) &&\
    cd /code

RUN mkdir /ext

ADD DMsimp_s_spin0 /code/MG5_aMC_v2_6_0/models/DMsimp_s_spin0
ADD DMsimp_s_spin1 /code/MG5_aMC_v2_6_0/models/DMsimp_s_spin1

# Install ROOT (required for Delphes)
RUN wget https://root.cern.ch/download/root_v6.10.02.source.tar.gz &&\
    tar xfz root_v6.10.02.source.tar.gz &&\
    mkdir build && cd build && cmake -DCMAKE_INSTALL_PREFIX=/tmp/root -Dx11=OFF ../root-6.10.02 &&\
    make && make install &&\
    rm -rf root_v6.10.02.source.tar.gz root-6.10.02 build

# Install delphes into the madgraph environment
RUN cd /code/MG5_aMC_v2_6_0 &&\
    (echo "install Delphes" | ./bin/mg5_aMC) &&\
    cd /code

ENTRYPOINT ["/code/MG5_aMC_v2_6_0/bin/mg5_aMC"]
