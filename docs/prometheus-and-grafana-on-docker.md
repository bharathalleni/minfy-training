## Installing Prometheus, Grafana and Node exporter on Docker

### Installation on Docker:

Requirements :
-   A server running Ubuntu 18.04 LTS.
-   Docker and Docker-compose installed 
-   A non-root user with sudo privileges.
-   TCP ports 9090 and 3000 opened

This is the simplest way of getting Prometheus and Grafana up and running. To install the entire monitoring stack using Docker compose 

```
git clone https://github.com/bharathalleni/minfy-training.git
cd minfy-training/docker/
docker-compose up -d
```



