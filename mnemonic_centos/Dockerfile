from centos
MAINTAINER Xiaojin Jiao email: xj.jiao@gmail.com

#proxy 
ENV PROXY_HOST ""
ENV PROXY_PORT ""
#ENV http_proxy $PROXY_HOST:$PROXY_PORT



#install necessary packages for spark building
RUN yum update -y && yum install -y \
	check \
	check-devel \
       	cmake \
       	git \
       	java-devel \
       	libuuid-devel\
       	tar\
       	wget ; \
       	yum clean all && yum groupinstall -y "Development tools"
 
ENV JAVA_HOME /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.111-2.6.7.2.el7_2.x86_64

WORKDIR root

#install Maven
RUN wget http://mirrors.ibiblio.org/apache/maven/maven-3/3.3.9/binaries/apache-maven-3.3.9-bin.tar.gz 
RUN tar xzvf apache-maven-3.3.9-bin.tar.gz && \
	mv apache-maven-3.3.9 /usr/local/apache-maven
RUN rm -f apache-maven-3.3.9-bin.tar.gz
ENV M2_HOME /usr/local/apache-maven
ENV M2 $M2_HOME/bin
ENV M2 $M2_HOME/bin
ENV PATH $M2:$PATH
#need to set up proxy with MAVEN_OPT

#install NVML
#libuuid-devel is required
RUN wget https://github.com/pmem/nvml/archive/0.1+b16.tar.gz
RUN tar xzvf 0.1+b16.tar.gz
RUN cd nvml-0.1-b16 && make install 
RUN rm -rf 0.1+b16.tar.gz nvml-0.1-b16


#install PMalloc
#check and check-devel, development tools are required
RUN git clone https://github.com/NonVolatileComputing/pmalloc.git
RUN cd pmalloc && mkdir build && cd build && cmake .. && make && make install 
RUN rm -rf pmalloc

#install mnemonic
RUN echo export MAVEN_OPTS="\" $([[ \"x\" != \"x${PROXY_HOST}\" ]] && echo -DproxySet=\\\"true\\\" -DproxyHost=${PROXY_HOST} -DproxyPort=${PROXY_PORT}) \"" \
	> /etc/profile.d/mvn.sh && chmod +x /etc/profile.d/mvn.sh
RUN git clone https://github.com/apache/incubator-mnemonic.git && \
	cd incubator-mnemonic && source /etc/profile.d/mvn.sh && mvn clean package install

#make a remote repo lined to my forked version
RUN cd incubator-mnemonic && git remote add origin-fork https://github.com/xjjiao/incubator-mnemonic.git
CMD ["bash"]
