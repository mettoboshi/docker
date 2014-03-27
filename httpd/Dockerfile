#
# VERSION 0.0.1
#

FROM centos:6.4
MAINTAINER mettoboshi 

# Setup
#RUN yum update -y
RUN yum install -y httpd

# Add 
ADD htdocs /var/www/html
RUN chown root:root -R /var/www/html

ADD conf/httpd.conf /etc/httpd/conf/httpd.conf
RUN chown root:root /etc/httpd/conf/httpd.conf


# ENV
ENV LANG ja_JP.utf8

# exporse
EXPOSE 80

# CMD
CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"] 
