#
# VERSION 0.0.1
#

FROM centos:6.4
MAINTAINER mettoboshi 

# Setup
RUN yum update -y
RUN yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel git gcc sqlite-devel readline-devel which

# Ruby
RUN git clone git://github.com/sstephenson/rbenv.git /usr/local/rbenv
RUN mkdir -p /usr/local/rbenv/shims
RUN mkdir -p /usr/local/rbenv/versions

RUN groupadd rbenv
RUN useradd rbenv -g rbenv
RUN chown -R rbenv:rbenv /usr/local/rbenv/
RUN git clone https://github.com/sstephenson/ruby-build.git /usr/local/rbenv/plugins/ruby-build

ADD conf/rbenv.sh /etc/profile.d/
RUN chown -R root:root /etc/profile.d/rbenv.sh
RUN source /etc/profile.d/rbenv.sh; rbenv install 2.0.0-p451 
RUN source /etc/profile.d/rbenv.sh; rbenv global 2.0.0-p451 
RUN source /etc/profile.d/rbenv.sh; rbenv rehash 
RUN source /etc/profile.d/rbenv.sh; gem install bundler 
RUN chmod +x /etc/profile.d/rbenv.sh

ENV RBENV_ROOT /usr/local/rbenv
ENV PATH $RBENV_ROOT/bin:$RBENV_ROOT/shims:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:~/.nodebrew/current/bin/$PATH
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

# Node.js
RUN curl https://raw.github.com/hokaccha/nodebrew/master/nodebrew | perl - setup
ENV PATH /.nodebrew/current/bin/:$PATH
RUN nodebrew install-binary latest 
RUN nodebrew use latest

# ENV
ENV LANG ja_JP.utf8

# install cloudconductor/maker 
RUN mkdir -p /var/www/develop/empty
RUN git clone https://github.com/cloudconductor/maker.git /var/www/develop/trial-maker
ADD conf/oauth.yml /var/www/develop/trial-maker/config/
RUN chown root:root /var/www/develop/trial-maker/config/oauth.yml
RUN su -; cd /var/www/develop/trial-maker; bundle install
RUN su -; cd /var/www/develop/trial-maker; bundle exec rake init 

# exporse
EXPOSE 8082
