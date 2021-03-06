# Licensed to the Apache Software Foundation (ASF) under one or more
# contributor license agreements.  See the NOTICE file distributed with
# this work for additional information regarding copyright ownership.
# The ASF licenses this file to You under the Apache License, Version 2.0
# (the "License"); you may not use this file except in compliance with
# the License.  You may obtain a copy of the License at
#
#     http://www.apache.org/licenses/LICENSE-2.0
#
# Unless required by applicable law or agreed to in writing, software
# distributed under the License is distributed on an "AS IS" BASIS,
# WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
# See the License for the specific language governing permissions and
# limitations under the License.

## yeongeon/centos:7
FROM centos
MAINTAINER "Yeongeon KIM" <yeongeon@gmail.com>

# epel
WORKDIR /tmp
RUN curl http://dl.fedoraproject.org/pub/epel/7/x86_64/e/epel-release-7-5.noarch.rpm -Lo epel-release-7-5.noarch.rpm \
  && rpm -ivh epel-release-7-5.noarch.rpm \
  && rm -f epel-release-7-5.noarch.rpm

# Yum
RUN yum update -x java* -y 
RUN yum -x java* install -v -y \
    tar which nc file passwd \
    gcc make gcc-c++ curl-devel expat-devel gettext-devel \
    openssl-devel zlib-devel perl-ExtUtils-MakeMaker bzip2 libcurl-devel \
    libcurl libtool aclocal autoheader automake autoconf cmake \
    openssh-server openssh-clients svn \
    libxml2 libxml2-devel libxslt libxslt-devel \
  && chkconfig sshd on \
  && mkdir /data

# Java
WORKDIR /tmp
RUN curl http://download.oracle.com/otn-pub/java/jdk/8u45-b14/jdk-8u45-linux-x64.tar.gz -LO -H "Cookie:oraclelicense=accept-securebackup-cookie" \
  && tar -xvf jdk-8u45-linux-x64.tar.gz \
  && mv jdk1.8.0_45 /usr/local/java \
  && rm -f /tmp/jdk-8u45-linux-x64.tar.gz \ 
  && echo 'JAVA_HOME="/usr/local/java"' >> /etc/environment

# Env
ENV JAVA_HOME=/usr/local/java
ENV PATH=$PATH:$JAVA_HOME/bin

# Maven
WORKDIR /tmp
RUN curl http://apache.mirror.cdnetworks.com/maven/maven-3/3.3.1/binaries/apache-maven-3.3.1-bin.tar.gz -LO \ 
  && tar -xvf apache-maven-3.3.1-bin.tar.gz \
  && mv apache-maven-3.3.1 /usr/local/maven \
  && echo 'M2_HOME=/usr/local/maven' >> /etc/environment \
  && echo 'M2=$M2_HOME/bin' >> /etc/environment \
  && echo 'PATH=$M2:$PATH' >> /etc/environment \
  && rm -f /tmp/apache-maven-3.3.1-bin.tar.gz 

ENV M2_HOME=/usr/local/maven
ENV M2=$M2_HOME/bin
ENV PATH=$M2:$PATH

# Git
WORKDIR /tmp
RUN curl https://www.kernel.org/pub/software/scm/git/git-2.3.6.tar.gz -Lo git-2.3.6.tar.gz \
  && tar -zxvf git-2.3.6.tar.gz \
  && cd /tmp/git-2.3.6 && ./configure --prefix=/usr/local/ && make prefix=/usr/local all && make prefix=/usr/local install \
  && rm -f /tmp/git-2.3.6.tar.gz \
  && rm -rf /tmp/git-2.3.6

# Python 27
WORKDIR /tmp
RUN curl https://www.python.org/ftp/python/2.7.8/Python-2.7.8.tgz -Lo Python-2.7.8.tgz \
  && tar -zxvf Python-2.7.8.tgz \
  && cd /tmp/Python-2.7.8 && ./configure --prefix=/usr/local && make && make altinstall \
  && rm -f /tmp/Python-2.7.8.tgz \
  && rm -rf /tmp/Python-2.7.8
WORKDIR /tmp
RUN curl https://pypi.python.org/packages/source/d/distribute/distribute-0.6.49.tar.gz --insecure -Lo distribute-0.6.49.tar.gz \
  && tar -zxvf distribute-0.6.49.tar.gz \
  && cd distribute-0.6.49 && python2.7 setup.py install \
  && rm -f /tmp/distribute-0.6.49.tar.gz \
  && rm -rf /tmp/distribute-0.6.49

# Protobuf v2.5.0
WORKDIR /tmp
RUN git clone -b v2.5.0 https://github.com/google/protobuf.git protobuf_src \
  && cd /tmp/protobuf_src && ./autogen.sh && ./configure --prefix=/usr/local && make && make install \
  && rm -rf /tmp/protobuf_src

