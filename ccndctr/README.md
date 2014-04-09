Usage
===========

- Clone github repository

    git clone https://github.com/mettoboshi/docker.git

- Docker Build

    cd ccndctr
    
    docker build -t xxxx/xxxx .
    
    docer run -i -p 49160:22 -p 49161:8081 -p 49162:8082 -t xxxx/xxxx /bin/bash
    
- Start Service

    service sshd start
    
    cd /var/www/develop/trial
    
    bundle exec rake server:start
    
    cd ../trial-maker
    
    bundle exec rake server:start
