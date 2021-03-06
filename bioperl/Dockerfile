FROM bioperl/bioperl

MAINTAINER Dave Tang <me@davetang.org>

RUN apt-get clean all && \
   apt-get update && \
   apt-get upgrade -y && \
   apt-get install -y \
   wget \
   git \
   build-essential \
   zlib1g-dev \
   libbz2-dev \
   libcurl4-gnutls-dev \
   liblzma-dev \
   libncurses5-dev \
   libssl-dev \
   libxml2-dev \
   vim \
   && apt-get clean all && \
   apt-get purge && \
   rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# libraries required by Bio::DB::Sam
RUN mkdir /src && \
   cd /src && \
   wget https://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2 && \
   tar -xjf samtools-0.1.19.tar.bz2 && \
   cd samtools-0.1.19 && \
   cat Makefile | sed -E 's/^(CFLAGS.*)/\1 -fPIC/' > blah && \
   mv blah Makefile && \
   make
ENV SAMTOOLS=/src/samtools-0.1.19/

# Miniconda and dependencies
RUN cd /tmp/ && \
   wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh && \
   bash Miniconda3-latest-Linux-x86_64.sh -b -p $HOME/miniconda3 && \
   /root/miniconda3/condabin/conda install -y -c bioconda bwa samtools picard mummer minimap2
ENV PATH=$PATH:/root/miniconda3/bin

RUN cpanm Parallel::ForkManager Bio::DB::Sam namespace::clean

RUN mkdir /root/perllib && \
   cd /root/perllib && \
   wget https://raw.githubusercontent.com/vcftools/vcftools/master/src/perl/Vcf.pm

RUN cd /root/ && \
   git clone https://github.com/MullinsLab/Bio-Cigar.git && \
   cd Bio-Cigar/ && \
   perl Makefile.PL && \
   make install

COPY test.pl /tmp/

