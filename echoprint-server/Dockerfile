FROM ubuntu:14.04

# Install Java 8
RUN apt-get install software-properties-common -y && \
  add-apt-repository ppa:webupd8team/java -y && \
  apt-get update && \
  echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections && \
  apt-get install oracle-java8-installer -y && \
  apt-get install oracle-java8-set-default -y

# Install dependencies
RUN apt-get install -y build-essential python git zlib1g-dev libbz2-dev wget supervisor maven2

# Install web.py
RUN apt-get install -y python-setuptools && \
  easy_install web.py && \
  easy_install pyechonest

# Install tokyo cabinet
RUN wget http://sourceforge.net/projects/tokyocabinet/files/tokyocabinet/1.4.32/tokyocabinet-1.4.32.tar.gz && \
  tar xvf tokyocabinet-1.4.32.tar.gz && \
  mkdir /usr/local/tokyocabinet-1.4.32/ && \
  cd tokyocabinet-1.4.32 && ./configure --prefix=/usr/local/tokyocabinet-1.4.32/ && make && make install

# Install Tokyo Tyrant
RUN wget http://sourceforge.net/projects/tokyocabinet/files/tokyotyrant/1.1.33/tokyotyrant-1.1.33.tar.gz && \
  tar xvf tokyotyrant-1.1.33.tar.gz && \
  mkdir /usr/local/tokyotyrant-1.1.33 && \
  cd tokyotyrant-1.1.33 && ./configure --prefix=/usr/local/tokyotyrant-1.1.33/ --with-tc=/usr/local/tokyocabinet-1.4.32/ && make && make install && \
  cd /usr/local/tokyotyrant-1.1.33/ && ln -s /usr/local/tokyocabinet-1.4.32/lib/libtokyocabinet.so.8 lib/

WORKDIR /usr/local

# Install Echoprint server
RUN git clone git://github.com/AudioAddict/echoprint-server.git echoprint-server && \
  cd echoprint-server/solr/solr && \
  mkdir logs

# Build Hashr
RUN cd echoprint-server/hashr && \
  mvn clean package

# Setup supervisord config
ADD echoprint.conf /etc/supervisor/conf.d/echoprint.conf

EXPOSE 8080

CMD ["/usr/bin/supervisord"]
