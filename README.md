# haproxy-and-neo4j
Example config and helper script for front-ending a Neo4j instance with HAProxy

Example /etc/crontab entry:
```
0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/letsencrypt.sh renew c360.sisu.io
```



## Helpful commands
```
haproxy -c -f haproxy.cfg
[ALERT] 306/155904 (60603) : parsing [haproxy.cfg:50] : 'bind *:443' : unable to load SSL certificate file '/etc/ssl/c360.sisu.io/c360.sisu.io.pem' file does not exist.
[ALERT] 306/155904 (60603) : Error(s) found in configuration file : haproxy.cfg
[ALERT] 306/155904 (60603) : Fatal errors found in configuration.
```



### HAproxy Overview
* HAProxy is a reverse proxy. Basically a type of proxy server that retrieves resources on behalf of a client from some number of backend servers. 
* Each section in haproxy.cfg  basically pipes to the next section
* Four Sections: Global,Default,frontend,backend

#### Global



### Things I do not understand...


1. What is certbot being used for 
2. 
