FROM centos:centos7.9.2009
MAINTAINER The CentOS Project <cloud-ops@centos.org
#python3 container
ENV container docker
LABEL RUN="docker run -it --name NAME --privileged --ipc=host --net=host --pid=host -e HOST=/host -e NAME=NAME -e IMAGE=IMAGE -v /sys/fs/selinux:/sys/fs/selinux:ro -v /run:/run -v /var/log:/var/log -v /etc/localtime:/etc/localtime -v /:/host IMAGE"

RUN [ -e /etc/yum.conf ] && sed -i '/tsflags=nodocs/d' /etc/yum.conf || true

# Swap out the systemd-container package and install all useful packages
RUN yum -y install \
           gcc \
           gdb \
           git \
           glibc-common \
           glibc-utils \
           tar \
           docker \
           python-docker-py \
           docker-selinux \
           python-pip \
           python-devel \
           python2-pip \
           python-setuptools \
           python3 \
           python3-pip \
           python-virtualenv \
           python3-devel \
           libselinux-python3 \
           kubernetes-client \
           kubernetes-node \
           kubernetes-devel \
           kubernetes-master \
           gdb-gdbserver \
           vim-minimal \
           python-rhsm \
           && yum clean all

#mysql server stuff
RUN rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
RUN rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
RUN sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo
RUN yum -y --enablerepo=mysql57-community install \ 
           mysql-devel \
           mysql \
           mysql-common \
           mysql-libs \
           mysql-community-devel


RUN git clone https://github.com/ifriedman7/IFotos.git
WORKDIR IFotos
RUN /usr/bin/pip3 install -r requirements.txt


# Set default command
#CMD ["/usr/bin/bash"]
EXPOSE 3000
ENTRYPOINT [ "python3", "app.py" ]
