#############################################################
# Dockerfile to build tool container for encode-map
#############################################################

# Use 14.04 because gshuf output is different in later versions
FROM ubuntu:14.04

# Distributed by the ENCODE Data Coordinating Center
MAINTAINER J. Seth Strattan <jseth@stanford.edu>

# Setup prerequisites
USER root
RUN apt-get update && apt-get install -y \
	build-essential \
	git \
	libncurses5-dev \
	libncursesw5-dev \
	openjdk-7-jdk \
	python-dev \
	python-setuptools \
	unzip \
	wget \
	zlib1g-dev \
&& rm -rf /var/lib/apt/lists/*

# Setup packages
WORKDIR /image_software
# Trimmomatic
RUN wget http://www.usadellab.org/cms/uploads/supplementary/Trimmomatic/Trimmomatic-0.36.zip \
	&& unzip Trimmomatic-0.36.zip \
	&& rm Trimmomatic-0.36.zip
# bwa
RUN git clone --branch 0.7.10 https://github.com/lh3/bwa.git bwa_0_7_10 \
	&& (cd bwa_0_7_10 && make)
# ENCODE pipeline container repo
ARG CACHEBUST=3
RUN git clone https://github.com/strattan/encode-map.git encode-map
RUN groupadd -r ubuntu \
	&& useradd --no-log-init --create-home --system -g ubuntu --home /home/ubuntu ubuntu
USER ubuntu
WORKDIR /home/ubuntu

ENTRYPOINT ["python", "/image_software/encode-map/src/encode_map.py"]
#CMD ["/bin/bash -l"]