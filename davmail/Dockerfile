FROM ubuntu:18.04
MAINTAINER Super Wayne <super.x.wayne@gmail.com>

ENV VERSION 8

RUN apt-get update && apt-get install curl unzip software-properties-common -y && add-apt-repository ppa:openjdk-r/ppa -y && apt-get update && \
	apt-get install openjdk-"${VERSION}"-jdk -y --no-install-recommends && \
	apt-get autoclean && apt-get --purge -y autoremove && \
	rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /davmail && mkdir -p /davconf && cd /davmail && curl --silent --retry 3 https://svwh.dl.sourceforge.net/project/davmail/davmail/4.9.0/davmail-4.9.0-2652.zip -o davmail.zip && unzip davmail.zip && rm davmail.zip && chmod +x /davmail/davmail.sh
RUN cd /davmail && keytool -genkey -keyalg rsa -keysize 2048 -storepass password -keystore davmail.p12 -storetype \
                    pkcs12 -validity 3650 -dname cn=davmailhostname.company.com,ou=davmail,o=sf,o=net
ADD ./davmail.properties /davconf/

ENV JAVA_HOME /usr/lib/jvm/java-${VERSION}-openjdk-amd64

VOLUME ["/davconf"]
# caldav
EXPOSE 1080
#imap
EXPOSE 1143
#ldap
EXPOSE 1389
#pop
EXPOSE 1110
#smtp
EXPOSE 1025
#WORKDIR /davmail
CMD ["/davmail/davmail.sh","/davconf/davmail.properties"]
