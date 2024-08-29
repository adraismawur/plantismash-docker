FROM python:2.7

# glimmer first
RUN wget https://ccb.jhu.edu/software/glimmer/glimmer302b.tar.gz; \
    tar -xzf glimmer302b.tar.gz; \
    cd glimmer3.02/src; \
    make; \
    cd /; \
    # glimmerhmm
    wget https://ccb.jhu.edu/software/glimmerhmm/dl/GlimmerHMM-3.0.4.tar.gz; \
    tar -xzf GlimmerHMM-3.0.4.tar.gz; \
    # hmmer2
    wget http://eddylab.org/software/hmmer/hmmer-2.3.2.tar.gz; \
    tar -xzf hmmer-2.3.2.tar.gz; \
    cd hmmer-2.3.2; \
    ./configure; \
    make; \
    cd /; \
    # hmmer3
    wget http://eddylab.org/software/hmmer/hmmer-3.1b2.tar.gz; \
    tar -xzf hmmer-3.1b2.tar.gz; \
    cd hmmer-3.1b2; \
    ./configure; \
    make; \
    # install this one normally. bins in /usr/local/bin
    make install; \
    cd /; \
    # diamond
    wget https://github.com/bbuchfink/diamond/releases/download/v0.7.10/diamond-linux64.tar.gz; \
    tar -xzf diamond-linux64.tar.gz; \
    mv diamond bin; \
    # blast+
    wget https://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.2.31/ncbi-blast-2.2.31+-x64-linux.tar.gz; \
    tar -xzf ncbi-blast-2.2.31+-x64-linux.tar.gz; \
    # fasttree
    wget -P bin http://www.microbesonline.org/fasttree/FastTree; \
    # prodigal
    wget -P bin https://github.com/hyattpd/Prodigal/releases/download/v2.6.3/prodigal.linux; \
    # link hmmer2 binaries
    ln hmmer-2.3.2/src/hmmalign bin/hmmalign2; \
    ln hmmer-2.3.2/src/hmmbuild bin/hmmbuild2; \
    ln hmmer-2.3.2/src/hmmcalibrate bin/hmmcalibrate2; \
    ln hmmer-2.3.2/src/hmmconvert bin/hmmconvert2; \
    ln hmmer-2.3.2/src/hmmemit bin/hmmemit2; \
    ln hmmer-2.3.2/src/hmmfetch bin/hmmfetch2; \
    ln hmmer-2.3.2/src/hmmindex bin/hmmindex2; \
    ln hmmer-2.3.2/src/hmmpfam bin/hmmpfam2; \
    ln hmmer-2.3.2/src/hmmsearch bin/hmmsearch2; \
    # link glimmerhmm
    ln /GlimmerHMM/bin/glimmerhmm_linux_x86_64 bin/glimmerhmm; \
    # simple apt installs
    apt update; \
    apt install -y default-jre extract muscle prodigal liblzma-dev libxml2-dev redis; \
    # code
    wget https://github.com/plantismash/plantismash/archive/refs/heads/master.zip; \
    unzip master.zip; \
    mv plantismash-master plantismash; \ 
    cd plantismash; \
    pip install -r requirements.txt; \
    cd /; \
    # dispatcher
    wget https://github.com/plantismash/dispatcher/archive/refs/heads/master.zip -O dispatcher.zip; \
    unzip dispatcher.zip; \
    mv dispatcher-master dispatcher; \
    cd dispatcher; \
    pip install -r requirements.txt; \
    # replace with old redis version
    pip install redis==2.10.6 


ENV PATH=/glimmer3.02/bin:/hmmer-2.3.2:/hmmer-3.1b2:/ncbi-blast-2.2.31+:/plantismash:$PATH

WORKDIR /dispatcher

STOPSIGNAL SIGINT

CMD runSMASH

EXPOSE 8950
