FROM ubuntu:bionic
MAINTAINER Einar <elanfranco@cert.unlp.edu.ar>
MAINTAINER Jeremias <jpretto@cert.unlp.edu.ar>

ENV DEBIAN_FRONTEND noninteractive
RUN apt update && apt install -y python3-pip python3-dnspython python3-psutil python3-redis python3-requests python3-termstyle python3-tz python3-dateutil jq python3-sleekxmpp python3-pymongo python3-psycopg2 bash-completion

RUN pip3 install intelmq

#RUN mv `python3 -c "import site; print(site.getsitepackages()[0] + '/opt/intelmq')"` /opt/
RUN useradd -d /opt/intelmq -U -s /bin/bash intelmq
RUN intelmqsetup

RUN chmod -R 0770 /opt/intelmq
RUN chown -R intelmq.intelmq /opt/intelmq


RUN apt update -y && apt install -y apache2 php git sudo libapache2-mod-php7.2  && apt-get autoremove -y && apt-get clean

RUN git clone https://github.com/certtools/intelmq-manager.git /tmp/intelmq-manager \
 && cp -R /tmp/intelmq-manager/intelmq-manager/* /var/www/html/ \
&& chown -R www-data.www-data /var/www/html/ && rm /var/www/html/index.html

RUN usermod -a -G intelmq www-data && mkdir /opt/intelmq/etc/manager/ && \
touch /opt/intelmq/etc/manager/positions.conf && \
chgrp www-data /opt/intelmq/etc/*.conf /opt/intelmq/etc/manager/positions.conf && \
chmod g+w /opt/intelmq/etc/*.conf /opt/intelmq/etc/manager/positions.conf

RUN echo "www-data ALL=(intelmq) NOPASSWD: /usr/local/bin/intelmqctl" >> /etc/sudoers

ADD state.json /opt/intelmq/var/lib/state.json
ADD etc/defaults.conf /opt/intelmq/etc/
ADD etc/BOTS /opt/intelmq/etc/
ADD etc/pipeline.conf /opt/intelmq/etc/
ADD etc/runtime.conf /opt/intelmq/etc/
RUN chown -R intelmq.intelmq /opt/intelmq/ /opt/intelmq/var/lib/state.json
ADD dev_intelmq /opt/dev_intelmq
ADD entrypoint /usr/bin/entrypoint
#RUN cd /opt/dev_intelmq && git pull
RUN chmod +x /usr/bin/entrypoint
ENTRYPOINT ["entrypoint"]
CMD ["apachectl -DFOREGROUND"]
