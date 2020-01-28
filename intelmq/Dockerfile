FROM ubuntu:bionic
MAINTAINER Einar <elanfranco@cert.unlp.edu.ar>
MAINTAINER Jeremias <jpretto@cert.unlp.edu.ar>

ENV DEBIAN_FRONTEND noninteractive
RUN apt update && apt install -y python3-pip python3-dnspython python3-psutil python3-redis python3-requests python3-termstyle python3-tz python3-dateutil jq python3-sleekxmpp python3-pymongo python3-psycopg2 bash-completion

RUN pip3 install intelmq

RUN useradd -d /opt/intelmq -U -s /bin/bash intelmq
RUN intelmqsetup

RUN chmod -R 0770 /opt/intelmq

RUN apt update -y && apt install -y apache2 php git sudo libapache2-mod-php7.2  && apt-get autoremove -y && apt-get clean

RUN git clone https://github.com/certtools/intelmq-manager.git /tmp/intelmq-manager \
 && cp -R /tmp/intelmq-manager/intelmq-manager/* /var/www/html/ \
&& chown -R www-data.www-data /var/www/html/ && rm /var/www/html/index.html

RUN usermod -a -G intelmq www-data && mkdir /opt/intelmq/etc/manager

RUN echo "www-data ALL=(intelmq) NOPASSWD: /usr/local/bin/intelmqctl" >> /etc/sudoers

ADD state.json /opt/intelmq/var/lib/state.json
RUN git clone https://github.com/certtools/intelmq.git /opt/dev_intelmq

#ADD dev_intelmq /opt/dev_intelmq
ADD entrypoint /usr/bin/entrypoint
ADD update /usr/bin/update

RUN chmod +x /usr/bin/entrypoint
RUN for file in $(find /opt/dev_intelmq/ -name "*REQUIREMENTS.txt"); do pip3 install -r $file; done && touch /usr/local/lib/python3.6/dist-packages/test-easy-install-16.write-test && chown -R intelmq:intelmq /opt/intelmq/
RUN rm -fr /opt/intelmq/etc/*.conf 

ENTRYPOINT ["entrypoint"]
CMD ["apachectl -DFOREGROUND"]
