from centos
MAINTAINER Xiaojin Jiao email: xj.jiao@gmail.com

#proxy 
ENV PROXY_HOST ""
ENV PROXY_PORT ""
ENV http_proxy $PROXY_HOST:$PROXY_PORT



WORKDIR ~
RUN yum update -y && yum install -y git java-devel wget ; yum clean all
 
#RUN #Download JAVA JDK 8.0
#RUN wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u60-b27/jdk-8u60-linux-x64.rpm" 
#install JAVA 8 JDK
#RUN yum -y localinstall jdk-8u60-linux-x64.rpm
#ENV JAVA_HOME /usr/java/jdk1.8.0_60 
ENV JAVA_HOME /usr/lib/jvm/java-1.7.0-openjdk-1.7.0.111-2.6.7.2.el7_2.x86_64
#RUN rm -f ./jdk-8u60-linux-x64.rpmk

#install spark 1.6
RUN git clone git://github.com/apache/spark.git -b branch-1.6

#proxy settings for mvn
ADD settings.xml /root/.m2/settings.xml

WORKDIR ./spark
RUN build/mvn -DproxyHost=$PROXY_HOST -DproxyPort=$PROXY_PORT -DskipTests clean package

CMD ["bash"]
