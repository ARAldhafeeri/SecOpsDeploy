# SecOpsDeploy
The following solution utlize opensource projects such as nginx, mod security, Grafana Prometheus to apply resilient security to deployed web apps. 

## Architecture

![Architecture](./architecture.png)

## Reverse proxy & Mod Security layer 
- Nginx : Nginx as a reverse proxy and load balancer
- Mod Security: Mod Security as a WAF
- Mod Security Rules: OWASP Core Rule Set (CRS) v3.3.0

The purpose of this layer is to protect the web application from attacks such as SQL injection, XSS, etc.

## Apps layer
- Apps: Your web applications 
- Databases: Your databases
- Queues: protecting your queues managment ui

The purpose of this layer is to protect the web application from attacks such as SQL injection, XSS, etc.

## Observability layer
- Prometheus: Prometheus as a time series database
- Loki: Loki as a log aggregator
- Grafana: Grafana as a visualization tool
- Alert Manager: Alert Manager as an alerting tool
- Node Exporter: Node Exporter as a host metrics collector
- Promtail : Promtail as a client for like , log collector from various nodes

## collect logs from apps layer :
1. install loki driver on docker host
    ```sh
    docker plugin install grafana/loki-docker-driver:latest --alias loki --grant-all-permissions
    ```
2. add following configuration to docker service:
```
  labels:
      - "com.docker.compose.service=nginx"
      - "com.docker.compose.project=secopsdeploy"
    logging:
      driver: loki
      options:
        loki-url: http://localhost:3100/loki/api/v1/push
        loki-external-labels: job=nginx,owner=admin,environment=development
```
labels will be used to filter logs in loki
logging driver will send logs to loki

## push logs from remote host to loki:
1. install promtail on remote host
2. add following configuration to promtail.yml
```
server:
  http_listen_port: 9080
  grpc_listen_port: 0

positions:
    filename: /tmp/positions.yaml

clients:
    - url: http://loki:3100/loki/api/v1/push
```


## configuration 

### nginx configuration
- nginx.conf : configure nginx here 
- conf/conf.d : add your web apps configuration here

### mod security configuration
- modsecurity.conf : configure mod security here
- main.conf : configure mod security rules here
- conf/oswap_crs : clone repository checkout inside /conf/modsec, checkout  v3/master and rename root folder to oswap_crs
```sh
https://github.com/coreruleset/coreruleset.git
```


## Prerequisites
- Docker


## Installation

1. Clone the repo
   ```sh
   git clone
    ```
2. Run docker-compose
    ```sh
    docker compose build
    ```
3. Run docker-compose
    ```sh
    docker compose up
    ```
4. Access Grafana
    ```sh
    http://grafana.localhost
    ```
5. add datasrouce to grafana
    ```sh
    http://prometheus:9090
    http://loki:3100
    ```
6. Import WAF dashboard
    ```sh
    https://grafana.com/grafana/dashboards/<TBD>
    ```



Upgrade




Sign in

secopsdeploynginx
secopsdeploy_nginx
RUNNING





2024/01/06 12:39:00 [notice] 1#1: ModSecurity-nginx v1.0.3 (rules loaded inline/local/remote: 0/940/0)

{"@timestamp":"2024-01-06T12:39:04+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.004","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/","referer":"-","args":"orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","upstreamtime":"0.000","responsetime":"0.009","request_method":"GET","status":"200","size":"43374","request_body":"-","request_length":"837","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.002","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"735","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"735","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/user/orgs","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"47","request_body":"-","request_length":"784","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/user/orgs","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=5&type=dash-db","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"802","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/plugins","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"embedded=0&core=0","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"800","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/plugins","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=1","upstreamtime":"0.000","responsetime":"0.004","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"789","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"803","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"803","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30","upstreamtime":"0.010","responsetime":"0.004","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"790","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:14+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&script=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"790","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:15+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:15+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:16+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:19+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/","referer":"-","args":"orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","upstreamtime":"0.010","responsetime":"0.005","request_method":"GET","status":"200","size":"43367","request_body":"-","request_length":"808","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"732","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"732","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/user/orgs","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"47","request_body":"-","request_length":"781","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/user/orgs","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=5&type=dash-db","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"799","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/plugins","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"embedded=0&core=0","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"797","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/plugins","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=1","upstreamtime":"0.000","responsetime":"0.005","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"786","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30&starred=true","upstreamtime":"0.010","responsetime":"0.004","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"800","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30","upstreamtime":"0.010","responsetime":"0.004","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"787","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"800","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:20+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?orgId=1&foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"limit=30","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"787","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:21+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.002","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:21+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:24+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/","referer":"-","args":"foo=%3Cscript%3Ealert(1)%3C/script%3E","upstreamtime":"0.000","responsetime":"0.006","request_method":"GET","status":"200","size":"43367","request_body":"-","request_length":"800","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"724","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"724","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/user/orgs","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"47","request_body":"-","request_length":"787","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/user/orgs","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=5&type=dash-db","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"805","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/plugins","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"embedded=0&core=0","upstreamtime":"0.000","responsetime":"0.004","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"803","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/plugins","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"793","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30&starred=true","upstreamtime":"0.010","responsetime":"0.007","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"806","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=1","upstreamtime":"0.010","responsetime":"0.008","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"792","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"806","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:25+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30","upstreamtime":"0.000","responsetime":"0.005","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"793","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:27+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:32+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/","referer":"-","args":"foo=%3Cscript%3Ealert(1)%3C/script%3E","upstreamtime":"0.010","responsetime":"0.005","request_method":"GET","status":"200","size":"43367","request_body":"-","request_length":"800","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.000","responsetime":"0.004","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"724","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/dashboards/home","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert(1)%3C/script%3E","args":"-","upstreamtime":"0.010","responsetime":"0.002","request_method":"GET","status":"200","size":"1437","request_body":"-","request_length":"724","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/dashboards/home","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/user/orgs","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"47","request_body":"-","request_length":"732","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/user/orgs","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=5&type=dash-db","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"750","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/plugins","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"embedded=0&core=0","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"748","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/plugins","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"751","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=1","upstreamtime":"0.000","responsetime":"0.006","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"737","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30&starred=true","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"751","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"738","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:33+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/search","referer":"http://grafana.localhost/?foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","args":"limit=30","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"2","request_body":"-","request_length":"738","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/search","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:34+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:38+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"grafana.localhost","url":"/api/live/ws","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"400","size":"12","request_body":"-","request_length":"610","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/api/live/ws","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/","referer":"-","args":"foo=%3Cscript%3Ealert%281%29%3C%2Fscript%3E&orgId=1","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"302","size":"29","request_body":"-","request_length":"713","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/login","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"35159","request_body":"-","request_length":"765","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/login","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/runtime.5f6c48afe38620502c9b.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.002","request_method":"GET","status":"200","size":"13416","request_body":"-","request_length":"659","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/runtime.5f6c48afe38620502c9b.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/7653.f5c70a70add3b711f560.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"403125","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/7653.f5c70a70add3b711f560.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/grafana.dark.b44253d019cd9cb46428.css","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.005","request_method":"GET","status":"200","size":"213169","request_body":"-","request_length":"679","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/grafana.dark.b44253d019cd9cb46428.css","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/8781.91ede282a7f6078508e7.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"20558","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/8781.91ede282a7f6078508e7.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/app.54d9d7943f7448e4edcd.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"1028718","request_body":"-","request_length":"655","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/app.54d9d7943f7448e4edcd.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6580.3f973f26169425d3bb3b.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.010","request_method":"GET","status":"200","size":"4495693","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6580.3f973f26169425d3bb3b.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/runtime.5f6c48afe38620502c9b.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"13416","request_body":"-","request_length":"485","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/runtime.5f6c48afe38620502c9b.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/368.08fa689dcaf8b1f563ad.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.012","request_method":"GET","status":"200","size":"3255403","request_body":"-","request_length":"655","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/368.08fa689dcaf8b1f563ad.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/7653.f5c70a70add3b711f560.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.003","request_method":"GET","status":"200","size":"403125","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/7653.f5c70a70add3b711f560.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:40+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/8781.91ede282a7f6078508e7.js","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.005","request_method":"GET","status":"200","size":"20558","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/8781.91ede282a7f6078508e7.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/img/grafana_icon.svg","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"5690","request_body":"-","request_length":"702","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/img/grafana_icon.svg","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/fonts/inter/UcC73FwrK3iLTeHuS_fvQtMwCp50KnMa1ZL7.woff2","referer":"http://localhost/public/build/grafana.dark.b44253d019cd9cb46428.css","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"37056","request_body":"-","request_length":"742","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/fonts/inter/UcC73FwrK3iLTeHuS_fvQtMwCp50KnMa1ZL7.woff2","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6580.3f973f26169425d3bb3b.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"4495693","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6580.3f973f26169425d3bb3b.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/app.54d9d7943f7448e4edcd.js","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.004","request_method":"GET","status":"200","size":"1028718","request_body":"-","request_length":"481","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/app.54d9d7943f7448e4edcd.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/368.08fa689dcaf8b1f563ad.js","referer":"-","args":"-","upstreamtime":"0.010","responsetime":"0.005","request_method":"GET","status":"200","size":"3255403","request_body":"-","request_length":"481","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/368.08fa689dcaf8b1f563ad.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/7424.bcc638d570de088ab39a.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"39228","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/7424.bcc638d570de088ab39a.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6533.3b9380133f767d118419.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.002","request_method":"GET","status":"200","size":"107235","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6533.3b9380133f767d118419.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/AngularApp.7b2c19a14daecb80b16d.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"72824","request_body":"-","request_length":"662","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/AngularApp.7b2c19a14daecb80b16d.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/1188.27f7e331b07ff148c3e9.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.003","request_method":"GET","status":"200","size":"37088","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/1188.27f7e331b07ff148c3e9.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6783.b14a1b4e00f7499447fb.js","referer":"http://localhost/login","args":"-","upstreamtime":"0.010","responsetime":"0.004","request_method":"GET","status":"200","size":"188975","request_body":"-","request_length":"656","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6783.b14a1b4e00f7499447fb.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/img/g8_login_dark.svg","referer":"http://localhost/login","args":"-","upstreamtime":"0.000","responsetime":"0.001","request_method":"GET","status":"200","size":"2361","request_body":"-","request_length":"703","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/img/g8_login_dark.svg","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/img/fav32.png","referer":"http://localhost/login","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"1118","request_body":"-","request_length":"695","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/img/fav32.png","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/7424.bcc638d570de088ab39a.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"39228","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/7424.bcc638d570de088ab39a.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/1188.27f7e331b07ff148c3e9.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.001","request_method":"GET","status":"200","size":"37088","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/1188.27f7e331b07ff148c3e9.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6533.3b9380133f767d118419.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"107235","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6533.3b9380133f767d118419.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/AngularApp.7b2c19a14daecb80b16d.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"72824","request_body":"-","request_length":"488","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/AngularApp.7b2c19a14daecb80b16d.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:41+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/public/build/6783.b14a1b4e00f7499447fb.js","referer":"-","args":"-","upstreamtime":"0.000","responsetime":"0.002","request_method":"GET","status":"200","size":"188975","request_body":"-","request_length":"482","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/public/build/6783.b14a1b4e00f7499447fb.js","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

{"@timestamp":"2024-01-06T12:39:53+00:00","host":"2df44d20b970","ID":"nginxmainlog","server_ip":"172.20.0.5","client_ip":"172.20.0.1","xff":"-","domain":"localhost","url":"/login","referer":"-","args":"foo=%3Cscript%3Ealert(10)%3C/script%3E","upstreamtime":"0.000","responsetime":"0.005","request_method":"GET","status":"200","size":"35152","request_body":"-","request_length":"804","protocol":"HTTP/1.1","upstreamhost":"172.20.0.4:3000","file_dir":"/etc/nginx/html/login","http_user_agent":"Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.0.0 Safari/537.36"}

Search...
Stick to bottom