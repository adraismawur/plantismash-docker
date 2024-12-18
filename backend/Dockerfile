FROM mambaorg/micromamba

WORKDIR /home/$MAMBA_USER

USER root

COPY --chown=$MAMBA_USER:$MAMBA_USER environment.yml /home/$MAMBA_USER/environment.yml

RUN apt update
RUN apt install -y wget
RUN apt install -y unzip
RUN apt install -y gcc


USER $MAMBA_USER

RUN micromamba install -y -n base -f /home/$MAMBA_USER/environment.yml && \
    micromamba clean --all --yes

ARG MAMBA_DOCKERFILE_ACTIVATE=1

## DEPENDENCIES ##

# glimmer first
# RUN wget https://ccb.jhu.edu/software/glimmer/glimmer302b.tar.gz; \
#     tar -xzf glimmer302b.tar.gz; \
#     cd glimmer3.02/src; \
#     make; \
#     cd /; 

# # glimmerhmm
# RUN wget https://ccb.jhu.edu/software/glimmerhmm/dl/GlimmerHMM-3.0.4.tar.gz; \
#     tar -xzf GlimmerHMM-3.0.4.tar.gz;

# # hmmer2
# RUN wget http://eddylab.org/software/hmmer/hmmer-2.3.2.tar.gz; \
#     tar -xzf hmmer-2.3.2.tar.gz; \
#     cd hmmer-2.3.2; \
#     ./configure; \
#     make; \
#     cd /;

# # hmmer3
# RUN wget http://eddylab.org/software/hmmer/hmmer-3.1b2.tar.gz; \
#     tar -xzf hmmer-3.1b2.tar.gz; \
#     cd hmmer-3.1b2; \
#     ./configure; \
#     make; \
#     # install this one normally. bins in /usr/local/bin
#     make install; \
#     cd /;

# # diamond
# RUN wget https://github.com/bbuchfink/diamond/releases/download/v0.7.10/diamond-linux64.tar.gz; \
#     tar -xzf diamond-linux64.tar.gz; \
#     mv diamond bin;

# # blast+
# RUN wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.2.31/ncbi-blast-2.2.31+-x64-linux.tar.gz; \
#     tar -xzf ncbi-blast-2.2.31+-x64-linux.tar.gz;

# # fasttree
# RUN wget -P bin http://www.microbesonline.org/fasttree/FastTree;

# # prodigal
# RUN wget -P bin https://github.com/hyattpd/Prodigal/releases/download/v2.6.3/prodigal.linux;

# # link hmmer2 binaries
# RUN ln hmmer-2.3.2/src/hmmalign bin/hmmalign2; \
#     ln hmmer-2.3.2/src/hmmbuild bin/hmmbuild2; \
#     ln hmmer-2.3.2/src/hmmcalibrate bin/hmmcalibrate2; \
#     ln hmmer-2.3.2/src/hmmconvert bin/hmmconvert2; \
#     ln hmmer-2.3.2/src/hmmemit bin/hmmemit2; \
#     ln hmmer-2.3.2/src/hmmfetch bin/hmmfetch2; \
#     ln hmmer-2.3.2/src/hmmindex bin/hmmindex2; \
#     ln hmmer-2.3.2/src/hmmpfam bin/hmmpfam2; \
#     ln hmmer-2.3.2/src/hmmsearch bin/hmmsearch2;

# # link glimmerhmm
# RUN ln /GlimmerHMM/bin/glimmerhmm_linux_x86_64 bin/glimmerhmm;

# # simple apt installs
# RUN apt update;
# RUN apt install -y default-jre extract muscle prodigal liblzma-dev libxml2-dev redis;



# code
RUN wget https://github.com/plantismash/plantismash/archive/refs/tags/2.0-beta2.zip; \
    unzip 2.0-beta2.zip; \
    mv plantismash-2.0-beta2 plantismash; \ 
    cd plantismash;

WORKDIR /home/$MAMBA_USER/plantismash

RUN pip install -r requirements.txt
RUN pip install graphlan
RUN python download_databases.py;

# clusterblast. TODO: remove once this is integrated into clusterblast itself

WORKDIR /home/$MAMBA_USER

# dispatcher
RUN wget https://github.com/plantismash/dispatcher/archive/refs/heads/master.zip -O dispatcher.zip; \
    unzip dispatcher.zip; \
    mv dispatcher-master dispatcher; \
    cd dispatcher; \
    pip install -r requirements.txt;

# replace with old redis version
RUN pip install redis==2.10.6 

# ENV PATH=/glimmer3.02/bin:/hmmer-2.3.2:/hmmer-3.1b2:/ncbi-blast-2.2.31+:/plantismash:$PATH

WORKDIR /home/$MAMBA_USER/dispatcher

STOPSIGNAL SIGINT

CMD redis-server --daemonize yes --port 8951 --protected-mode no; \
    ./runSMASH -q redis://localhost:8951/0 --workdir /data/plantismash/upload --container

EXPOSE 8951
