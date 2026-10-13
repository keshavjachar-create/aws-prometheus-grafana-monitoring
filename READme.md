\# AWS EC2 Monitoring with Prometheus and Grafana



\## 1. Project Overview



This project implements a server monitoring solution on AWS EC2 using Docker, Prometheus, Node Exporter, and Grafana.



The objective is to collect Linux system metrics, store them in Prometheus, and visualize system performance through Grafana dashboards.



\## 2. Technologies Used



\- AWS EC2

\- Amazon Linux 2023

\- Docker

\- Docker Compose

\- Prometheus

\- Node Exporter

\- Grafana

\- PromQL

\- MobaXterm and SSH tunneling



\## 3. Architecture



1\. AWS EC2 provides the Linux environment.

2\. Docker Compose runs the monitoring containers.

3\. Node Exporter exposes Linux system metrics.

4\. Prometheus scrapes and stores the metrics.

5\. Grafana queries Prometheus and displays dashboards.

6\. SSH tunnels provide private access to the Prometheus and Grafana web interfaces.



\## 4. Project Configuration



The configuration files are stored in the `configuration` folder.



\- `docker-compose.yml` — Defines the monitoring services, container settings, ports, and persistent volumes.

\- `prometheus.yml` — Defines Prometheus scrape intervals and monitoring targets.



\### Prometheus scrape targets



\- `prometheus:9090` — Prometheus server metrics.

\- `node-exporter:9100` — Linux system metrics.



The configured scrape interval is 30 seconds.



\## 5. Monitoring Features



The Grafana Node Exporter Full dashboard displays:



\- CPU utilization

\- Memory usage

\- Filesystem usage

\- Network traffic

\- System uptime

\- Disk and system metrics



\## 6. PromQL Queries



\### Check target health



```promql

up

```



A value of 1 indicates a successful scrape.



\### CPU usage percentage



```promql

100 \* (1 - avg by (instance) (

&#x20; rate(node\_cpu\_seconds\_total{mode="idle"}\[5m])

))

```



\### Memory usage percentage



```promql

100 \* (1 - node\_memory\_MemAvailable\_bytes /

node\_memory\_MemTotal\_bytes)

```



\### Root filesystem usage percentage



```promql

100 \* (1 -

&#x20; node\_filesystem\_avail\_bytes{mountpoint="/",fstype!=""}

&#x20; /

&#x20; node\_filesystem\_size\_bytes{mountpoint="/",fstype!=""}

)

```



\## 7. Useful Troubleshooting Commands



Check container status:



```bash

docker compose ps

```



View container resource consumption:



```bash

docker stats --no-stream

```



Check system memory:



```bash

free -h

```



View Prometheus logs:



```bash

docker compose logs --tail=100 prometheus

```



View Grafana logs:



```bash

docker compose logs --tail=100 grafana

```



Check Prometheus health:



```bash

curl -s http://127.0.0.1:9090/-/healthy

```



Check Grafana health:



```bash

curl -s http://127.0.0.1:3000/api/health

```



\## 8. Challenges and Solutions



\### Prometheus was not accessible in the browser



Used MobaXterm SSH tunneling to forward the local browser connection to the EC2 instance.



\### Prometheus showed no query results



Executed PromQL queries to validate target health and verify metric collection.



\### CPU results were difficult to interpret



Used a PromQL expression that calculates CPU usage from the rate of CPU idle time.



\### Grafana required a Prometheus data source



Configured the data-source URL as `http://prometheus:9090` and verified it using Save \& test.



\### Grafana consumed substantial memory



Investigated system and container memory usage with `free -h` and `docker stats`. Reduced unnecessary dashboard activity and disabled automatic refresh.



\## 9. Security Considerations



\- Prometheus and Grafana ports are bound to localhost on the EC2 instance.

\- SSH tunnels are used to access the web interfaces.

\- Monitoring ports should not be exposed publicly without appropriate security controls.

\- Passwords, private keys, and other credentials must not be committed to source control.



\## 10. Project Outcome



Successfully deployed and verified a Docker-based monitoring stack on AWS EC2. Confirmed metric collection through PromQL and visualized CPU, memory, filesystem, network, and uptime information using Grafana.



\## 11. Future Improvements



\- Configure CPU and memory alerts.

\- Publish the project configuration and documentation to GitHub after removing secrets.

\- Add dashboard screenshots.

\- Explore monitoring multiple EC2 instances.



