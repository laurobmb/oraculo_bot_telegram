FROM centos:centos7
MAINTAINER Lauro de Paula 

LABEL www.laurodepaula.com.br="10.0.0-beta"
LABEL vendor="Bot Python"

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
ENV TZ=America/Recife

RUN yum -y update && \
	yum -y install epel-release && \
	yum -y install python-pip && \
	yum -y install python36 && \
	yum -y groupinstall "Development Tools" && \
	yum -y install python36-pip.noarch && \
	yum clean all

ADD . /bot/
	
RUN python3 -m pip install --upgrade pip && \
	cd /bot; python3 -m pip install -r requirements.txt

WORKDIR /bot/

CMD ["python3", "/bot/bot.py"]
