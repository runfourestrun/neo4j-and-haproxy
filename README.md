# haproxy-and-neo4j
Example config and helper script for front-ending a Neo4j instance with HAProxy

Example /etc/crontab entry:
```
0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/letsencrypt.sh renew c360.sisu.io
```


## Thoughts:



not as simple as writing the contents of your haproxy.cfg (https://github.com/voutilad/haproxy-and-neo4j)  to /etc/haproxy/haproxy.cfg on the host. The reason I'm skeptical is because in your haproxy.cfg file there are a lot of references to letsencrypt. I know when we setup the loadbalancer we had google create the certificate and it was issued by GTS and google assigns that cert to the LB (and updates the cert for us). I'm assuming GTS would need to be used in this Second Layer (HAProxy) as well... not letsencrypt. On the other hand, after reading a little into HAProxy, it seems your frontend http-in, and frontend tls-in sections have a fall back from use_backend: letsencrypt which is default_backend: neo4j-http. The default_backend neo4j-http only looks like it would be used for http traffic though so .... 


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
