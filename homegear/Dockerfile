FROM debian:stretch
#FROM resin/rpi-raspbian:jessie

# File Author / Maintainer
#MAINTAINER Sven Berger
MAINTAINER Mario Wicke

############################################################
# don't ask for stupid things 
############################################################
ENV DEBIAN_FRONTEND noninteractive 
#RUN echo APT::Install-Recommends "0"; >> /etc/apt/apt.conf 
#RUN echo APT::Install-Suggests "0"; >> /etc/apt/apt.conf

############################################################
# Environment Variables
############################################################
ENV LC_ALL de_DE.UTF-8
ENV LANG de_DE.UTF-8
ENV LANGUAGE de_DE.UTF-8
ENV USERNAME homegear
ENV USERID 9001
ENV GROUPID 9001
ENV TZ Europe/Berlin

############################################################
# Update Repository and upgrade Software
############################################################

RUN apt-get update
RUN apt-get dist-upgrade -y
RUN apt-get install wget \
mc \
locales \
apt-transport-https \
ca-certificates \
apt-utils \
gnupg \
-y

############################################################
# Konfigure timezone and locale
############################################################

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone 
#RUN ln -s /etc/locale.alias /usr/share/locale/locale.alias
RUN dpkg-reconfigure locales && locale-gen de_DE.UTF-8 && /usr/sbin/update-locale de_DE.UTF-8
RUN echo 'de_DE.UTF-8 UTF-8' >> /etc/locale.gen ; locale-gen de_DE.UTF-8; echo LANG="de_DE.UTF-8" >> /etc/default/locale

############################################################
# User anlegen
############################################################
RUN groupadd -g $GROUPID $USERNAME &&\
    adduser --disabled-password --gecos '' --home /opt/$USERNAME --uid $USERID --gid $GROUPID $USERNAME &&\
    adduser $USERNAME dialout &&\
    adduser $USERNAME users &&\
    adduser $USERNAME sudo &&\
    adduser $USERNAME $USERNAME


############################################################
# Add Repository and upgrade Software
############################################################
RUN wget https://apt.homegear.eu/Release.key && apt-key add Release.key && rm Release.key
RUN echo 'deb https://apt.homegear.eu/Debian/ stretch/' >> /etc/apt/sources.list.d/homegear.list
RUN apt-get update && apt-get dist-upgrade -y

############################################################
# Install Software
# first Homegear
############################################################
RUN apt-get install homegear \
homegear-nodes-core \
-y
############################################################
# then modules
############################################################
RUN apt-get install \
#homegear-max \
homegear-homematicbidcos \
#homegear-homematicwired \
#homegear-insteon \
#homegear-philipshue \
#homegear-sonos \
#homegear-kodi \
#homegear-ipcam \
#homegear-beckhoff \
#homegear-knx \
#homegear-enocean \
#homegear-intertechno \
#aspell-de \
-y

############################################################
# Copy External Data
############################################################
RUN mkdir /etc/homegear.config && cp -r /etc/homegear/* /etc/homegear.config
RUN mkdir /var/lib/homegear.data && cp -r /var/lib/homegear/* /var/lib/homegear.data

############################################################
# Copy Start Scripts
############################################################
COPY start.sh /usr/local/bin/start.sh
RUN chmod +x /usr/local/bin/start.sh

############################################################
# Remove Apt Data
############################################################
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

############################################################
# copy basicConf
############################################################
COPY ./basicConf/homematicbidcos.conf /etc/homegear/config/families/homematicbidcos.conf

############################################################
# Folders needed
############################################################
VOLUME /etc/homegear 
VOLUME /var/lib/homegear
VOLUME /var/log/homegear


############################################################
# Prots needed
############################################################

EXPOSE 2001 2002 2003


############################################################
# Container Start
############################################################
#CMD ["/bin/true"]
#CMD ["service pilight start"]
#CMD ["/bin/bash", "service pilight start"]
#CMD ["/bin/bash", "/usr/local/sbin/pilight-debug"]
#CMD ["/bin/bash", "/usr/local/sbin/pilight-daemon"]
CMD ["/bin/bash", "/usr/local/bin/start.sh"]