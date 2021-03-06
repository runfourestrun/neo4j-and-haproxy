# haproxy-and-neo4j
Example config and helper script for front-ending a Neo4j instance with HAProxy

Example /etc/crontab entry:
```
0 0,12 * * * root python -c 'import random; import time; time.sleep(random.random() * 3600)' && /usr/local/bin/letsencrypt.sh renew c360.sisu.io
```



HAproxy articles:

* https://www.haproxy.com/blog/haproxy-ssl-termination/
* https://serversforhackers.com/c/letsencrypt-with-haproxy

## Thoughts:



not as simple as writing the contents of Dave's haproxy.cfg (https://github.com/voutilad/haproxy-and-neo4j)  to /etc/haproxy/haproxy.cfg on the GCP VM. I know when we setup the loadbalancer we had google create the certificate and it was issued by GTS and google assigns that cert to the LB (and updates the cert for us). I'm assuming GTS would need to be used in this Second Layer (HAProxy) as well... not letsencrypt. 


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




### Things I do not understand...

I need the SSL certificate on the Load Balancer (SSL Termination). I think the SSL certificate also needs to be formatted in a funky way (it expects the SSL certificate to all be in one file which includes the certificate chain, root cert and private key). The shell script handles this automatically using letsencrypt but they are not the issuer in this situation. 

### line in haproxy.cfg: Frontend_tls
```	bind *:443 ssl crt /etc/ssl/c360.sisu.io/c360.sisu.io.pem ```

### Running this command raises the following errors. 
```
haproxy -c -f haproxy.cfg
[ALERT] 306/155904 (60603) : parsing [haproxy.cfg:50] : 'bind *:443' : unable to load SSL certificate file '/etc/ssl/c360.sisu.io/c360.sisu.io.pem' file does not exist.
[ALERT] 306/155904 (60603) : Error(s) found in configuration file : haproxy.cfg
[ALERT] 306/155904 (60603) : Fatal errors found in configuration.
```

### thoughts
1. I don't need to use certbot since letsencrypt is not issuing the cert but GTS is.. Certbot seems to be an commandline program to generate certs from letsencrypt.
2. I think at minimum the shell script needs to be heavily refactored for GTS. I think the script is still functionally neccessary because it looks like it installs the cert on the host which I think still needs to happen irregardless of the cert issuer... Unless I do it manually. 
3. ```	bind *:443 ssl crt /etc/ssl/c360.sisu.io/c360.sisu.io.pem ```
4. Basically just really struggling with using TLS termination with HAProxy (reverse proxy)

