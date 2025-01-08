FROM quay.io/centos/centos:stream8

USER root

# Modify all yum repository configurations to use vault.centos.org
RUN for i in $(ls /etc/yum.repos.d); do sed -i -e "s/mirrorlist.*//g" /etc/yum.repos.d/$i &&     sed -i -e  \
    "s/#baseurl=http:\/\/mirror.centos.org/baseurl=http:\/\/vault.centos.org/g" /etc/yum.repos.d/$i; done   

# Install necessary packages
RUN dnf update -y && \
    dnf module enable -y pki-core pki-deps postgresql:12 && \
    dnf install -y gcc make java-17-openjdk-devel gettext openssl tomcat wget unzip git systemd postgresql python3 python3-libxml2 python3-requests jss && \
    dnf clean all

ENV JAVA_HOME=/usr/lib/jvm/java-17-openjdk
ENV JRE_HOME=/usr/lib/jvm/jre-17-openjdk
ENV PATH="${JAVA_HOME}/bin:${PATH}"
ENV DBHOSTNAME="postgresdb"

RUN ln -s /usr/bin/python3 /usr/bin/python

RUN sed -i 's|^JAVA_HOME=.*|JAVA_HOME=/usr/lib/jvm/java-17-openjdk|' /etc/tomcat/tomcat.conf

# WORKDIR /opt
# RUN wget -O jprofiler.tar.gz "https://download.ej-technologies.com/jprofiler/jprofiler_linux_14_0_5.tar.gz" \
#     && tar -xzf jprofiler.tar.gz \
#     && rm jprofiler.tar.gz

# ENV JPROFILER_HOME /opt/jprofiler14.0.5

# ENV JAVA_TOOL_OPTIONS="-agentpath:$JPROFILER_HOME/bin/linux-x64/libjprofilerti.so=port=8849"

WORKDIR /candlepin

CMD ["/usr/sbin/init"]
