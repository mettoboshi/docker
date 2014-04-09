#
# VERSION 0.0.1
#

FROM centos:6.4
MAINTAINER mettoboshi 

# Setup
RUN yum install -y openssh openssh-server openssh-clients passwd
RUN groupadd developers
RUN useradd mettoboshi -g developers
RUN passwd -f -u mettoboshi
ADD conf/changepw.txt /tmp/
RUN chpasswd < /tmp/changepw.txt

# ENV
ENV LANG ja_JP.utf8

# exporse
EXPOSE 22

# CMD
CMD /usr/sbin/sshd -D 
