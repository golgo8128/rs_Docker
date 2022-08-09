FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y wget
RUN apt-get install -y build-essential
RUN apt-get install -y git
RUN apt-get install -y libgsl-dev
RUN apt-get install -y r-base

RUN apt-get install -y vim
RUN apt-get install -y emacs

RUN Rscript -e 'install.packages(c("magrittr", "readr", "stringr", "plyr", "dplyr", "data.table"))'
RUN Rscript -e 'install.packages("argparser")'
RUN Rscript -e 'if (!require("BiocManager", quietly = TRUE)){ install.packages("BiocManager") };  BiocManager::install(c("GenomicRanges", "BSgenome"), ask = F)'

RUN apt-get install -y bioperl
RUN apt-get install -y cpanminus
RUN cpanm Getopt::Long
RUN cpanm Text::CSV
RUN cpanm List::MoreUtils
RUN cpanm Data::GUID
RUN cpanm Interpolation
RUN cpanm IPC::System::Simple
RUN cpanm Storable
RUN cpanm Switch::Plain
RUN cpanm File::Which

RUN apt-get install -y bowtie2

RUN mkdir TranslocPipeline
WORKDIR TranslocPipeline

RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/extTools.tgz
RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/genomes.tgz
RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/transloc_pipeline.tgz
RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/Workspace.tgz
RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/set_transloc_path1.sh
RUN wget --no-check-certificate https://krebs.iab.keio.ac.jp/~rsaito/Shared/TranslocPipeline/20220809/set_transloc_refgenome1.sh

RUN tar xvfz extTools.tgz
RUN rm extTools.tgz
RUN tar xvfz genomes.tgz
RUN rm genomes.tgz
RUN tar xvfz transloc_pipeline.tgz
RUN rm transloc_pipeline.tgz
RUN tar xvfz Workspace.tgz
RUN rm Workspace.tgz

WORKDIR /TranslocPipeline/extTools/SeqPrep
RUN make
ENV PERL5LIB /TranslocPipeline/extTools/ea-utils/ExpressionAnalysis-ea-utils-10c2192/clipper
WORKDIR /TranslocPipeline/extTools/ea-utils/ExpressionAnalysis-ea-utils-10c2192/clipper
RUN make
WORKDIR /TranslocPipeline/extTools/samtools-0.1.17
RUN make

ENV SAMTOOLS /TranslocPipeline/extTools/samtools-0.1.17/
RUN cpanm Bio::DB::Bam

# CMD source /TranslocPipeline/set_transloc_path1.sh
ENV PATH /TranslocPipeline/transloc_pipeline/bin:/TranslocPipeline/transloc_pipeline/R:/TranslocPipeline/extTools/SeqPrep:/TranslocPipeline/extTools/ea-utils/ExpressionAnalysis-ea-utils-10c2192/clipper:/TranslocPipeline/extTools/samtools-0.1.17:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV GENOME_DB /TranslocPipeline/genomes
ENV BOWTIE2_INDEXES /TranslocPipeline/genomes/bowtie2_indexes

WORKDIR /TranslocPipeline/Workspace/tutorial_data
