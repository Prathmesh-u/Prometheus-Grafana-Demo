# Prometheus & Grafana Monitoring - Implementation Steps

This document provides **detailed, step-by-step instructions** to implement observability and monitoring for a containerized Flask web application using **Prometheus** and **Grafana** on AWS.

---

## Step 0: Prerequisites

Before starting, ensure you have:

- An AWS account
- Basic knowledge of:
  - Linux commands
  - Docker & Docker Compose
  - Prometheus & Grafana basics
- An SSH key pair for EC2 access
- Security Groups allowing required ports

---

## Step 1: Launch EC2 Instances

Launch **two EC2 instances** using **Amazon Linux**.

### 1️⃣ Web Server EC2
This instance hosts:
- Flask web application
- Node Exporter
- cAdvisor

**Open the following ports in the Security Group:**

| Port | Purpose |
|-----|--------|
| 5000 | Flask application |
| 9100 | Node Exporter |
| 8080 | cAdvisor |
| 22 | SSH |

---

### 2️⃣ Monitoring EC2
This instance hosts:
- Prometheus
- Grafana

**Open the following ports in the Security Group:**

| Port | Purpose |
|-----|--------|
| 9090 | Prometheus |
| 3000 | Grafana |
| 22 | SSH |

---

## Step 2: SSH into the Web Server Instance

```bash
ssh -i <your-key.pem> ec2-user@<WEB_SERVER_PUBLIC_IP>
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

## Step 3: Deploy Flask Application (Web Server)

> Ensure your Flask app is running using Docker Compose.
docker compose up -d

Verify:
App is running on http://<WEB_SERVER_PUBLIC_IP>:5000
> Containers are healthy:
docker ps

## Step 4: Deploy Node Exporter & cAdvisor (Web Server)

docker run -d \
  --name=node-exporter \
  -p 9100:9100 \
  prom/node-exporter

docker run -d \
  --name=cadvisor \
  -p 8080:8080 \
  gcr.io/cadvisor/cadvisor:latest

OR YOU CAN ALSO USE DOCKER COMPOSE FOR IT

> Verify exporters:
http://<WEB_SERVER_PUBLIC_IP>:9100/metrics
http://<WEB_SERVER_PUBLIC_IP>:8080

## Step 5: SSH into Monitoring Server

ssh -i <your-key.pem> ec2-user@<MONITORING_SERVER_PUBLIC_IP>

> Install Docker and Docker Compose:
sudo yum update -y
sudo yum install docker -y
sudo systemctl start docker
sudo systemctl enable docker
sudo usermod -aG docker ec2-user

> Install Docker Compose plugin:
sudo mkdir -p /usr/local/lib/docker/cli-plugins
sudo curl -SL https://github.com/docker/compose/releases/download/v2.29.7/docker-compose-linux-x86_64 \
-o /usr/local/lib/docker/cli-plugins/docker-compose
sudo chmod +x /usr/local/lib/docker/cli-plugins/docker-compose
docker compose version

> Add the prometheus.yml & docker-compose.yml file to deploy and configure prometheus and grafana

## Step 8: Verify Prometheus Targets

> Open Prometheus:
http://<MONITORING_SERVER_PUBLIC_IP>:9090 check STATUS > TARGETS Ensure: Node Exporter → UP, cAdvisor → UP

> Configure Grafana
http://<MONITORING_SERVER_PUBLIC_IP>:3000 
Default credentials:
Username: admin
Password: admin

> Add Prometheus Data Source
Go to Connections → Data Sources
Select Prometheus
URL: http://prometheus:9090
Save & Test

## Step 9: Create Dashboards

> Import Node Exporter Dashboard
- Dashboard ID: 1860
- Import from Grafana dashboard library

> Create Custom Panels
- CPU usage
- Memory usage
- Disk usage
- Container metrics (cAdvisor)

Use PromQL for advanced queries.

## Step 10: Generate Traffic for Metrics

> To observe real-time metrics:
for i in {1..100}; do
  curl http://<WEB_SERVER_PUBLIC_IP>:5000
done

You should now see spikes in CPU, memory, and container metrics.

## Step 11: Verification Checklist

- ✅ Flask app accessible
- ✅ Node Exporter metrics visible
- ✅ cAdvisor metrics visible
- ✅ Prometheus targets UP
- ✅ Grafana dashboards updating in real time

## Outcome

> Prometheus scrapes host and container metrics
> Grafana visualizes system health and performance
> Full observability achieved for: Host, Containers, Application traffic