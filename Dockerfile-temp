FROM debian:12.5

ARG 1cversion=8.3.23.1865

RUN apt-get update && \
    apt-get upgrade && \
    apt-get install -y ca-certificates wget locales &&\
    apt-get install -y apache2 &&\
    apt-get install -y fontconfig imagemagick &&\
    apt-get install -y xfonts-utils cabextract &&\
    wget --quiet --output-document /tmp/ttf-mscorefonts-installer_3.6_all.deb http://ftp.de.debian.org/debian/pool/contrib/m/msttcorefonts/ttf-mscorefonts-installer_3.8.1_all.deb \
  && dpkg --install /tmp/ttf-mscorefonts-installer_3.6_all.deb 2> /dev/null \
  && rm /tmp/ttf-mscorefonts-installer_3.6_all.deb 

RUN a2dismod mpm_prefork || true && \
    a2dismod mpm_event || true && \
    a2enmod mpm_worker || true
RUN  /etc/init.d/apache2 start 

RUN mkdir /var/www/Megapolis2023 && \
    mkdir /var/www/megapolis_new && \
    mkdir /var/www/megapolis_old && \
    mkdir /1c/ && \
    mkdir /1c/Megapolis2023

COPY ./soft/ /tmp/
RUN dpkg -i /tmp/1c-*  || true
RUN rm -rf /tmp/*
RUN ./opt/1cv8/x86_64/8.3.23.1865/webinst -publish -apache24 -wsdir Megapolis2023 -dir /var/www/Megapolis2023/ -connstr "File=""/1c/Megapolis2023/"";" -confpath /etc/apache2/apache2.conf
#RUN ./opt/1cv8/x86_64/8.3.23.1865/webinst -publish -apache24 -wsdir megapolis_new -dir /var/www/megapolis_new/ -connstr "File=""/1c/megapolis_new/"";" -confpath /etc/apache2/apache2.conf
#RUN ./opt/1cv8/x86_64/8.3.23.1865/webinst -publish -apache24 -wsdir megapolis_old -dir /var/www/megapolis_old/ -connstr "File=""/1c/megapolis_old/"";" -confpath /etc/apache2/apache2.conf
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
EXPOSE 80

