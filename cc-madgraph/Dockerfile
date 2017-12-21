FROM cern/cc7-base

RUN yum -y update

RUN yum -y install python2-devel root gcc g++ gcc-gfortran make openssl wget boost-devel python2-root python2-jupyroot which rsync

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
    rm ../MG5_aMC_v2.6.0.tar.gz

RUN cd /code/MG5_aMC_v2_6_0 &&\
    (echo "set lhapdf /usr/bin/lhapdf-config" | ./bin/mg5_aMC) &&\
    (echo "set fastjet /usr/bin/fastjet-config" | ./bin/mg5_aMC) &&\
    (echo "install pythia8" | ./bin/mg5_aMC) &&\
    (printf "generate p p > g g\noutput /code/test\nlaunch\ndone\ndone\n" | ./bin/mg5_aMC) &&\
    rm -rf /code/test &&\
    cd /code

RUN mkdir /ext

ADD DMsimp_s_spin0 /code/MG5_aMC_v2_6_0/models/DMsimp_s_spin0
ADD DMsimp_s_spin1 /code/MG5_aMC_v2_6_0/models/DMsimp_s_spin1

RUN yum -y install python2-nbformat root-montecarlo-eg

RUN cd /code/MG5_aMC_v2_6_0 && \
    (echo "install Delphes" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) 

RUN yum -y install python2-pip python-matplotlib && pip install jupyter

RUN yum -y install python2-jsmva root-tmva-gui root-tmva-python ImageMagick

RUN yum -y install cmake

RUN cd /code/MG5_aMC_v2_6_0 && \
    (echo "install cuttools" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) &&\
    (echo "install iregi" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) &&\
    (echo "install ninja" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) &&\
    (echo "install collier" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) &&\
    (echo "install golem" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC)

RUN cd /code/MG5_aMC_v2_6_0 && \
    (printf "generate g g > u u~ [QCD]\noutput ppgg\n" | ROOTSYS=/usr PATH=/usr/bin:$PATH ./bin/mg5_aMC) &&\
    rm -rf ppgg

# RUN  echo "pythia8_path = /code/MG5_aMC_v2_6_0/HEPTools/pythia8" >> /code/MG5_aMC_v2_6_0/input/mg5_configuration.txt

RUN pip install tensorflow keras
RUN pip install scikit-learn statsmodels

# RUN yum -y install bzip2 file

# RUN cd /code && wget http://www.hepforge.org/archive/sherpa/SHERPA-MC-2.2.4.tar.gz &&\
#     tar xfz SHERPA-MC-2.2.4.tar.gz && cd SHERPA-MC-2.2.4 &&\
#     ./configure --enable-hepmc2=/code/MG5_aMC_v2_6_0/HEPTools/hepmc --enable-lhapdf=/usr --enable-fastjet=/usr --with-sqlite3=install &&\
#     make install && cd .. && rm -rf SHERPA-MC-2.2.4.tar.gz SHERPA-MC-2.2.4

RUN yum -y install git

RUN cd /code && git clone https://github.com/nim-lang/Nim && cd Nim && git clone --depth 1 git://github.com/nim-lang/csources && cd csources && sh build.sh && cd .. && bin/nim c koch && ./koch boot -d:release && ./koch tools && ./koch nimble && rm -rf csources && ln -sf /code/Nim/bin/* /usr/bin && chmod go+x /code/Nim/bin/*

# For ROOT TBrowser, etc.
RUN yum -y install mesa-dri-drivers

CMD ["/code/MG5_aMC_v2_6_0/bin/mg5_aMC"]
