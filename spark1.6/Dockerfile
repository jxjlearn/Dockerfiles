from ubuntu
MAINTAINER Xiaojin Jiao email: xj.jiao@gmail.com

ENV PROXY_HOST "" 
ENV PROXY_PORT "" 
ENV http_proxy $PROXY_HOST:$PROXY_PORT
#ENV MAVEN_OPT ""
ENV MAVEN_OPT "-DproxyHost=192.168.1.10 -DproxyPort=3128"

RUN apt-get update && apt-get install -y \
  default-jdk \
  git \
  vim \
  wget

RUN git clone git://github.com/apache/spark.git -b branch-1.6
WORKDIR ./spark
RUN build/mvn -DskipTests clean package

CMD ["bash"]
