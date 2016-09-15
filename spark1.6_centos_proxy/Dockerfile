from centos
MAINTAINER Xiaojin Jiao email: xj.jiao@gmail.com

#proxy for yum install
#ENV http_proxy 192.168.1.10:3128

#install necessary packages for spark building
RUN yum update -y && yum install -y git java-devel wget ; yum clean all
 
ENV JAVA_HOME /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.111-2.6.7.2.el7_2.x86_64

#clone spark 1.6 source code
RUN git clone git://github.com/apache/spark.git -b branch-1.6

#proxy settings for mvn
#ADD settings.xml /root/.m2/settings.xml
ENV MAVEN_OPT -DproxySet=true -Dhttp.proxyHost=192.169.1.10 -Dhttp.proxyPort=3128

#build spark
WORKDIR ./spark
RUN build/mvn -DskipTests clean package
