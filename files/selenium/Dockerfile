FROM ubuntu:12.04

RUN apt-get update

RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl
RUN echo "deb http://security.ubuntu.com/ubuntu precise-security main" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y python-software-properties python unzip

# Java
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get install oracle-java7-installer -y

RUN wget http://selenium-release.storage.googleapis.com/2.43/selenium-server-standalone-2.43.1.jar
CMD ["java", "-jar", "selenium-server-standalone-2.43.1.jar", "-role", "hub"]
