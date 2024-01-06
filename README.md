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
