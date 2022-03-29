FROM centos:centos7.9.2009
MAINTAINER The CentOS Project <cloud-ops@centos.org
#Database container
ENV container docker
LABEL RUN="docker run -it --name NAME --privileged --ipc=host --net=host --pid=host -e HOST=/host -e NAME=NAME -e IMAGE=IMAGE -v /sys/fs/selinux:/sys/fs/selinux:ro -v /run:/run -v /var/log:/var/log -v /etc/localtime:/etc/localtime -v /:/host IMAGE"

RUN [ -e /etc/yum.conf ] && sed -i '/tsflags=nodocs/d' /etc/yum.conf || true

RUN rpm -Uvh --replacepkgs https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
# Swap out the systemd-container package and install all useful packages
RUN yum -y install \
           kernel \
           e2fsprogs \
           sos \
           crash \
           strace \
           ltrace \
           tcpdump \
           abrt \
           pcp \
           systemtap \
           perf \
           bc \
           blktrace \
           btrfs-progs \
           ethtool \
           file \
           findutils \
           gcc \
           gdb \
           git \
           glibc-common \
           glibc-utils \
           hwloc \
           iotop \
           iproute \
           iputils \
           less \
           pciutils \
           ltrace \
           mailx \
           man-db \
           nc \
           netsniff-ng \
           net-tools \
           numactl \
           numactl-devel \
           passwd \
           perf \
           procps-ng \
           psmisc \
           screen \
           strace \
           sysstat \
           systemtap-client \
           tar \
           tcpdump \
           xauth \
           which \
           ostree \
           rpm-ostree \
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
           python-rhsm \
           rootfiles \
           yum-utils \
           epel-release \
           htop \
           httpd \
           s3fs-fuse \
           fuse \
           && yum clean all


#mysql server stuff
RUN rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
RUN rpm -Uvh https://repo.mysql.com/mysql80-community-release-el7-3.noarch.rpm
RUN sed -i 's/enabled=1/enabled=0/' /etc/yum.repos.d/mysql-community.repo
RUN yum -y --enablerepo=mysql57-community install \ 
           mysql-community-server \
           mysql-devel \
           mysql \
           mysql-common \
           mysql-libs \
           mysql-community-devel

RUN mkdir /mnt/my-bucket && \
    mkdir /tmp/bucket-cache && \
    echo $AWSACCESSKEYID:$AWSSECRETACCESSKEY > ${HOME}/.passwd-s3fs && \
    cat ${HOME}/.passwd-s3fs && \
    chmod 600 ${HOME}/.passwd-s3fs
RUN s3fs ike-bucket-small /mnt/my-bucket -o use_cache=/tmp/bucket-cache -o passwd_file=${HOME}/.passwd-s3fs -o ibm_iam_auth -o allow_other

RUN git clone https://github.com/ifriedman7/IFotos.git
WORKDIR IFotos
RUN cp db/my.cnf /etc/ && cp db/fuse.conf /etc/ 

#RUN /usr/bin/pip3 install -r requirements.txt

# Set default command
#CMD ["/usr/bin/bash"]
EXPOSE 3306
RUN systemctl restart mysqld
