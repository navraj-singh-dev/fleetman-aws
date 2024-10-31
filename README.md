# Fleetman - Cloud-Native Vehicle Tracking System

## 🎯 Project Overview

Fleetman is a microservices-based vehicle tracking system deployed on [AWS EKS](https://aws.amazon.com/eks/), featuring real-time position tracking, monitoring, logging, and alerting capabilities. The project demonstrates cloud-native architecture patterns and DevOps practices using [Kubernetes](https://kubernetes.io/).

## 📺 Live Demo Video

**Please watch this live demo video, in this video the complete project was completely deployed live on AWS EKS and i was explaning it..**
[`Live Demo On Youtube`](https://youtu.be/6gsLzw5P_20)

## 🏗️ Architecture & Tech Stack

### Microservices Components

- Frontend Web Application (Angular-based)
- API Gateway Service
- Position Simulator Service
- Position Tracker Service
- [Apache ActiveMQ](https://activemq.apache.org/) Message Queue
- [MongoDB](https://www.mongodb.com/) Database

### Cloud & Infrastructure

- [Amazon EKS](https://aws.amazon.com/eks/) (Elastic Kubernetes Service)
- AWS EBS CSI Driver for persistent storage
- Docker containers (hosted on [DockerHub](https://hub.docker.com/))

### Monitoring & Observability

- [Prometheus](https://prometheus.io/) & [Grafana](https://grafana.com/) Stack
- [EFK](https://www.elastic.co/what-is/elk-stack) Stack ([Elasticsearch](https://www.elastic.co/elasticsearch/), [Fluentd](https://fluentbit.io/), [Kibana](https://www.elastic.co/kibana/))
- Alertmanager with Slack integration
- Custom alert configurations

## 🔑 Key Features

- Real-time vehicle position tracking simulation
- Microservices architecture
- Message queue-based communication
- Persistent storage with AWS EBS
- Monitoring with Prometheus/Grafana
- Logging with EFK(elasticsearch, fluentd, kibana) stack
- Slack alerts for cluster events
- Load balanced services
- Horizontal scaling capability

## 🚀 Deployment Guide

### Prerequisites

- AWS Account with EKS permissions
- `kubectl` configured
- `eksctl` installed
- [Helm](https://helm.sh/) 3.x (for reference charts)
- All the required manifest files for project

### Deployment Steps

1. Create EKS Cluster:

```bash
# Configure the manifest file first..

eksctl create cluster -f cluster-config/clusterConfig.yaml
```

2. Deploy Storage Class and MongoDB:

```bash
kubectl apply -f storage-aws.yaml
kubectl apply -f mongo-stack-aws.yaml
```

3. Deploy Application Components:

```bash
kubectl apply -f services-aws.yaml
kubectl apply -f workloads-aws.yaml
```

4. Deploy Monitoring Stack:

```bash
# Deploy Prometheus CRDs first
kubectl apply -f monitoring/crds.yaml

# Deploy monitoring stack
kubectl apply -f monitoring/eks-monitoring.yaml

```

5. Deploy EFK Stack for Logging:

```bash
# Deploy elasticsearch, fluentd, kibana at once..
kubectl apply -f efk/elastic-stack-aws.yaml


# Deploy the configuration fluntd needs to get and send properly..
kubectl apply -f efk/fluentd-config.yaml

```

6. Configure Alerting:

```bash
# Create secret for Alertmanager
kubectl create secret generic --from-file=alertmanager.yaml -n monitoring alertmanager-monitoring-kube-prometheus-alertmanager

# Apply test deployment if needed
kubectl apply -f alerting/error-deployment.yaml
```

## 📊 Monitoring & Logging Access

#### Grafana Dashboard
- Access via LoadBalancer service
- Default credentials in monitoring/grafana-password.txt
- Pre-configured dashboards for all components

#### Kibana Interface
- Access through LoadBalancer service
- Port: 5601
- Indexes automatically created by Fluentd

#### Alertmanager
- Configured with Slack integration
- Alerts visible in designated Slack channel
- Customizable alert rules

## 🔧 Configuration Notes
- EKS cluster runs 3 worker nodes for high availability
- Persistent volumes used for MongoDB and Elasticsearch
- Fluentd DaemonSet ensures log collection from all nodes
- Services exposed via LoadBalancer or ClusterIP as needed

## 📈 Scaling & Performance
- Horizontal pod autoscaling available
- EBS volumes for persistent data
- Load balanced frontend service
- Resource requests and limits defined

## 🛠️ Troubleshooting
- Check pod logs using kubectl logs
- Monitor resources through Grafana dashboards
- View application logs in Kibana
- Check Slack channel for alerts

## 👨‍💻 About ME

- LinkedIn: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[My LinkedIN Profile](https://www.linkedin.com/in/navraj-singh-78b746210/)
- X (Twitter): &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[My X Profile](https://x.com/NavrajSinghDev)
- Hasnode (My Technical Blogs): &nbsp;[My Blogs](https://navraj-blog.hashnode.dev/)
- Email: &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;navrajsingh.dev@gmail.com