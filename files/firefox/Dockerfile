FROM ubuntu:12.04

RUN apt-get update

RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl
RUN echo "deb http://security.ubuntu.com/ubuntu precise-security main" >> /etc/apt/sources.list
RUN apt-get update
RUN apt-get install -y python-software-properties python unzip libnss3 libgconf-2-4

# Java
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get install oracle-java7-installer -y

# Selenium
RUN wget http://selenium-release.storage.googleapis.com/2.43/selenium-server-standalone-2.43.1.jar

# Firefox
RUN apt-get install firefox xvfb -y


ADD register-node.sh /var/register-node.sh
RUN chmod 755 /var/register-node.sh
CMD ["/bin/bash", "/var/register-node.sh"]

RUN cp /etc/hosts /tmp/hosts
RUN mkdir -p -- /lib-override && cp /lib/x86_64-linux-gnu/libnss_files.so.2 /lib-override
RUN perl -pi -e 's:/etc/hosts:/tmp/hosts:g' /lib-override/libnss_files.so.2
ENV LD_LIBRARY_PATH /lib-override
RUN echo "172.17.42.1 rob.nxtgd.net" >> /tmp/hosts
