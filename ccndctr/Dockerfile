#
# VERSION 0.0.1
#

FROM centos:6.4
MAINTAINER mettoboshi 

# Setup
RUN yum update -y
RUN yum install -y curl-devel expat-devel gettext-devel openssl-devel zlib-devel git gcc sqlite-devel readline-devel openssh openssh-server openssh-client which passwd sudo lsof wget

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
ENV PATH $RBENV_ROOT/bin:$RBENV_ROOT/shims:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/.nodebrew/current/bin/:/var/www/develop/phantomjs/phantomjs-1.9.7-linux-x86_64/bin:$PATH
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc

# Node.js
RUN curl https://raw.github.com/hokaccha/nodebrew/master/nodebrew | perl - setup
ENV PATH /.nodebrew/current/bin/:$PATH
RUN nodebrew install-binary latest 
RUN nodebrew use latest
RUN npm install -g jshint 

# ENV
ENV LANG ja_JP.utf8

# Make Developer
RUN groupadd developers
RUN useradd mettoboshi -G developers,wheel
RUN passwd -f -u mettoboshi
ADD conf/changepw.txt /tmp/
ADD conf/sudoers /etc/
RUN chown root:root /etc/sudoers
RUN chmod 440 /etc/sudoers
RUN chpasswd < /tmp/changepw.txt
RUN echo 'gem: --no-rdoc --no-ri' >> ~/.gemrc
# sudoers
RUN echo "%wheel  ALL=(ALL) NOPASSWD: ALL" >> /etc/sudoers


# Create directory 
RUN mkdir -p /var/www/develop/empty
RUN chown -R mettoboshi:developers /var/www/develop

# Change User
USER mettoboshi
ENV HOME /home/mettoboshi

# install cloudconductor/maker
RUN git clone https://github.com/mettoboshi/maker.git /var/www/develop/trial-maker
ADD conf/oauth.yml /var/www/develop/trial-maker/config/
RUN chown mettoboshi:developers /var/www/develop/trial-maker/config/oauth.yml
USER root
RUN sudo su -; cd /var/www/develop/trial-maker; bundle install
RUN sudo su -; cd /var/www/develop/trial-maker; bundle exec rake init 
RUN sudo su -; cd /var/www/develop/trial-maker; RAILS_ENV=test bundle exec rake db:migrate 

RUN mkdir /var/www/develop/phantomjs
RUN cd /var/www/develop/phantomjs; wget https://bitbucket.org/ariya/phantomjs/downloads/phantomjs-1.9.7-linux-x86_64.tar.bz2
RUN cd /var/www/develop/phantomjs; tar xvjf phantomjs-1.9.7-linux-x86_64.tar.bz2

# Jasmin-phantomjs
RUN cd /var/www/develop/trial-maker/vendor/guard-jasmine-phantomjs/vendor; git clone https://github.com/larrymyers/jasmine-reporters.git

# install cloudconductor/gui
USER mettoboshi
RUN git clone https://github.com/mettoboshi/ui.git /var/www/develop/trial
ADD conf/bridge.yml /var/www/develop/trial/config/
RUN chown mettoboshi:developers /var/www/develop/trial/config/bridge.yml
USER root
RUN sudo su -; cd /var/www/develop/trial; bundle install
RUN sudo su -; cd /var/www/develop/trial; bundle exec rake init 
RUN chown -R mettoboshi:developers /var/www/develop/

# exporse
EXPOSE 8081 8082 22

