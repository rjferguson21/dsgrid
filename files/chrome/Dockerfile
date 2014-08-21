FROM ubuntu:12.04

RUN apt-get update

RUN dpkg-divert --local --rename --add /sbin/initctl
RUN ln -sf /bin/true /sbin/initctl
RUN echo "deb http://security.ubuntu.com/ubuntu precise-security main" >> /etc/apt/sources.list
RUN apt-get install -y python-software-properties python unzip libnss3 libgconf-2-4

# Java
RUN add-apt-repository ppa:webupd8team/java
RUN apt-get update
RUN echo debconf shared/accepted-oracle-license-v1-1 select true | debconf-set-selections
RUN echo debconf shared/accepted-oracle-license-v1-1 seen true | debconf-set-selections
RUN apt-get install oracle-java7-installer -y

# Selenium
RUN wget http://selenium-release.storage.googleapis.com/2.42/selenium-server-standalone-2.42.2.jar

# ChromeDriver
RUN wget http://chromedriver.storage.googleapis.com/2.10/chromedriver_linux64.zip
RUN unzip chromedriver_linux64.zip && rm chromedriver_linux64.zip && mv chromedriver /usr/bin && chmod 755 /usr/bin/chromedriver

# XVFB
RUN apt-get install xvfb -y

# Chrome
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg --install google-chrome-stable_current_amd64.deb || apt-get -f install -y
RUN rm -f /opt/google/chrome/chrome-sandbox
RUN wget https://googledrive.com/host/0B5VlNZ_Rvdw6NTJoZDBSVy1ZdkE -O /opt/google/chrome/chrome-sandbox
RUN chown root:root /opt/google/chrome/chrome-sandbox && chmod 4755 /opt/google/chrome/chrome-sandbox && md5sum /opt/google/chrome/chrome-sandbox
RUN useradd chrome -m -s /bin/bash && mv /selenium-server-standalone-2.42.2.jar /home/chrome && chown chrome:chrome /home/chrome/selenium-server-standalone-2.42.2.jar
RUN mkdir /.pki && chown chrome:chrome /.pki && mkdir /.local && chown chrome:chrome /.local

#USER chrome
WORKDIR /home/chrome
ADD run.sh /home/chrome/run.sh
RUN chmod 755 /home/chrome/run.sh
CMD ["/bin/bash", "/home/chrome/run.sh"]
