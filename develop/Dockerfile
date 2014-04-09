#
# VERSION 0.0.1
#

FROM centos:6.4
MAINTAINER mettoboshi 

# yum
RUN yum install -y ncurses-devel wget gcc vim git

# user add
RUN groupadd developers
RUN useradd mettoboshi -g developers

# tmux
RUN wget http://downloads.sourceforge.net/tmux/tmux-1.8.tar.gz -P /usr/local
RUN wget https://github.com/downloads/libevent/libevent/libevent-2.0.21-stable.tar.gz -P /usr/local

RUN cd /usr/local; tar zxvf libevent-2.0.21-stable.tar.gz
RUN cd /usr/local/libevent-2.0.21-stable; ./configure; make; make install
RUN cd /usr/local; tar zxvf tmux-1.8.tar.gz
RUN cd /usr/local/tmux-1.8; ./configure; make; make install
RUN ln -s /usr/local/lib/libevent-2.0.so.5 /usr/lib64/libevent-2.0.so.5

# dotfiles
USER mettoboshi
ENV HOME /home/mettoboshi
RUN git clone https://github.com/mettoboshi/dotfiles.git /home/mettoboshi/dotfiles
RUN chown -R mettoboshi:developers /home/mettoboshi/dotfiles
RUN cd /home/mettoboshi/dotfiles; ./setup.sh
#RUN su - mettoboshi; vim
# ENV
ENV LANG ja_JP.utf8

# exporse
#EXPOSE 80

# CMD
#CMD ["/usr/sbin/httpd", "-D", "FOREGROUND"] 
