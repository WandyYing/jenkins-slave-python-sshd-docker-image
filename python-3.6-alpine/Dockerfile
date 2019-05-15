FROM python:3.6.8-alpine
LABEL maintainer="Ying Jun <wandy1208@gmail.com>"

ARG PROJECT_PATH=/usr/src/python-app/
ENV SSH_SERVER_KEYS /etc/ssh/host_keys/

ENV JAVA_VERSION 8u191
ENV JAVA_ALPINE_VERSION 8.191.12-r0

RUN apk add --no-cache  && \
    apk add --no-cache git subversion mercurial wget curl unzip openssh ca-certificates procps bash openjdk8-jre="$JAVA_ALPINE_VERSION" && \
    rm -rf /var/cache/apk/*

ENV JAVA_HOME /usr/lib/jvm/java-1.8-openjdk/jre
ENV PATH $JAVA_HOME/bin:$JAVA_HOME/lib:$PATH

RUN addgroup -S -g 10000 jenkins && \
    adduser -S -u 10000 -h /home/jenkins -G jenkins jenkins



EXPOSE 22

RUN apk update && \
    apk add bash openssh sudo && \
    mkdir -p ${SSH_SERVER_KEYS} && \
    mkdir ${PROJECT_PATH} && \
    echo -e "HostKey ${SSH_SERVER_KEYS}ssh_host_rsa_key" >> /etc/ssh/sshd_config && \
    echo -e "HostKey ${SSH_SERVER_KEYS}ssh_host_dsa_key" >> /etc/ssh/sshd_config && \
    echo -e "HostKey ${SSH_SERVER_KEYS}ssh_host_ecdsa_key" >> /etc/ssh/sshd_config && \
    echo -e "HostKey ${SSH_SERVER_KEYS}ssh_host_ed25519_key" >> /etc/ssh/sshd_config && \
    sed -i "s/#PermitRootLogin.*/PermitRootLogin\ yes/" /etc/ssh/sshd_config && \
    echo "root:root" | chpasswd && \
    chmod a+x /usr/local/bin/* && \
    rm -rf /var/cache/apk/* /tmp/*


USER jenkins
# WORKDIR /home/jenkins
WORKDIR ${PROJECT_PATH}

CMD ["/bin/sh"]