from ubuntu
#make sure it is using bash shell
RUN rm /bin/sh && ln -s /bin/bash /bin/sh

MAINTAINER Xiaojin Jiao email: xj.jiao@gmail.com

#proxy 
ENV PROXY_HOST ""
ENV PROXY_PORT ""
#ENV http_proxy "$PROXY_HOST:$PROXY_PORT"
#ENV proxy off

RUN apt-get update && apt-get install -y \
	autoconf \
	build-essential \
	check \ 
	cmake \
        default-jdk \
        git \
	man-db \
	uuid-dev\
	pkg-config \
        wget \
        vim

WORKDIR root

#install maven
RUN wget http://mirrors.ibiblio.org/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz 
RUN tar xzvf apache-maven-3.3.9-bin.tar.gz && \
	mv apache-maven-3.3.9 /usr/local/apache-maven
RUN rm -f apache-maven-3.3.9-bin.tar.gz
ENV M2_HOME /usr/local/apache-maven
ENV M2 $M2_HOME/bin
ENV PATH $M2:$PATH

#install NVML
#uuid-dev, autoconf and pkg-config are required
RUN git clone https://github.com/pmem/nvml.git
RUN cd nvml && git checkout tags/0.1+b16 && make && make install 

#install PMalloc
#check is required
#cmake, build-essential are required
RUN git clone https://github.com/NonVolatileComputing/pmalloc.git
RUN cd pmalloc && mkdir build && cd build && cmake .. && make && \
	make install 

#install mnemonic
RUN bash && echo export MAVEN_OPTS="\" $( [[ \"x\" != \"x${PROXY_HOST}\" ]] && echo -DproxySet=\\\"true\\\" -DproxyHost=${PROXY_HOST} -DproxyPort=${PROXY_PORT}) \"" > /etc/profile.d/mvn.sh && chmod +x /etc/profile.d/mvn.sh

RUN git clone https://github.com/apache/incubator-mnemonic.git && \
	cd incubator-mnemonic && source /etc/profile.d/mvn.sh && mvn clean package install

#make a remote repo lined to my forked version
RUN cd incubator-mnemonic && git remote add origin-fork https://github.com/xjjiao/incubator-mnemonic.git

CMD ["bash"]	

#preparation

