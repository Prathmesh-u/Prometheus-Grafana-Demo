# Prometheus & Grafana Monitoring Demo 📊

This repository demonstrates **observability and monitoring for a containerized web application** using **Prometheus** and **Grafana** on AWS.

The project focuses on **collecting, scraping, and visualizing metrics** from a running Flask application and its host system, following real-world monitoring practices.

---

## What This Project Does

Using **Prometheus**, **Grafana**, **Node Exporter**, and **cAdvisor**, this project enables monitoring of:

- ✅ **Host-level metrics**
  - CPU usage
  - Memory usage
  - Disk and system health
- ✅ **Container-level metrics**
  - Docker container resource usage
- ✅ **Application-level visibility**
  - Traffic and responsiveness of a Flask web application

All metrics are visualized using **Grafana dashboards**.

---

## Architecture Overview

The monitoring setup consists of **two EC2 instances**:

### Web Server Instance
- Runs a **Flask application** (Docker Compose)
- Exposes:
  - **Node Exporter** → `9100`
  - **cAdvisor** → `8080`
  - **Flask App** → `5000`

### Monitoring Server Instance
- Runs:
  - **Prometheus** → `9090`
  - **Grafana** → `3000`
- Scrapes metrics from the Web Server

Prometheus pulls metrics from exporters, and Grafana visualizes them through dashboards.

---

## ⚙️ Tools & Technologies Used

- **AWS EC2 (Amazon Linux)**
- **Docker & Docker Compose**
- **Prometheus** – Metrics collection
- **Grafana** – Visualization & dashboards
- **Node Exporter** – Host metrics
- **cAdvisor** – Container metrics
- **Flask** – Sample web application

---

## Learning Outcomes

- **Understand observability fundamentals**
- **Learn metrics scraping with Prometheus**
- **Build Grafana dashboards using PromQL**
- **Monitor hosts, containers, and applications**
- **Implement real-world monitoring architecture**

---

## Future Enhancements

- **Add Alertmanager for alerts**
- **Secure Grafana with HTTPS**
- **Monitor multiple web nodes**
- **Integrate with Terraform & Ansible**
- **Add application-level custom metrics**

---

**Author : Prathmesh Udanshiv** 🏗️
