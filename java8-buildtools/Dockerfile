FROM ubuntu:16.04

RUN apt-get update && apt-get install -y wget curl jq make apt-transport-https ca-certificates curl software-properties-common

RUN echo oracle-java8-installer shared/accepted-oracle-license-v1-1 select true | /usr/bin/debconf-set-selections && add-apt-repository -y ppa:webupd8team/java && apt-get update && apt-get install -y oracle-java8-installer && apt-get install -y oracle-java8-set-default && javac -version

RUN wget -O rq.binary https://s3-eu-west-1.amazonaws.com/record-query/record-query/x86_64-unknown-linux-gnu/rq \
    && chmod +x rq.binary && mv rq.binary /usr/local/bin/rq

RUN curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add -
RUN apt-key fingerprint 0EBFCD88
RUN add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
RUN apt-get update && apt-get install -y docker-ce \
    && docker --version

RUN curl -L https://github.com/docker/compose/releases/download/1.18.0/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose \
    && chmod +x /usr/local/bin/docker-compose \
    && docker-compose --version

