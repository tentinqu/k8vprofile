# Production-Ready Multi-Tier Web App on Kubernetes

## Overview
This project demonstrates a **highly available**, **fault-tolerant** deployment of a containerized web application (Tomcat + MySQL + RabbitMQ + Memcached) on Kubernetes using kOps on AWS.

## Features
- Kubernetes cluster provisioned with kOps
- Secrets management for sensitive credentials
- Persistent storage with AWS EBS for MySQL
- Auto-healing and horizontal scaling
- External access via NGINX Ingress + AWS ELB
- Zone-aware node labeling for EBS compatibility

## Prerequisites
- AWS account with IAM permissions
- kOps CLI installed
- kubectl configured
- Domain name (configured in Route53/GoDaddy)
- SSH key pair

## Cluster Setup
```bash
kops create cluster \
--name=kubeapp.harshitch.xyz \
--state=s3://kopsbucket23021 \
--zones=us-east-1a,us-east-1b \
--node-count=2 \
--node-size=t3.small \
--control-plane-size=t3.medium \
--dns-zone=kubeapp.harshitch.xyz \
--node-volume-size=12 \
--control-plane-volume-size=12 \
--ssh-public-key ~/.ssh/id_rsa.pub
```
## Deployment
Apply Kubernetes manifests:
```bash
kubectl apply -f manifests/
```
## Verify services:

```bash
kubectl get all,ingress,pvc
```
## Directory Structure
```text
manifests/
├── appdeploy.yaml       # Tomcat application
├── appingress.yaml      # Ingress rules
├── appservice.yaml      # App service
├── dbdeploy.yaml        # MySQL deployment
├── dbpvc.yaml           # Persistent volume claim
├── dbservice.yaml       # DB service
├── mcdep.yaml           # Memcached deployment
├── mcservice.yaml       # Memcached service
├── rmqdeploy.yaml       # RabbitMQ deployment
├── rmqservice.yaml      # RabbitMQ service
├── secret.yaml          # Secrets configuration
```
## Troubleshooting
| Symptom               | Solution                      |
|-----------------------|-------------------------------|
| Ingress not routing   | Check ELB CNAME record        |
| Pods pending          | Verify resource limits        |
| DB init failures      | Check initContainer logs      |
| Service unreachable   | Validate label selectors      |

## Cleanup
```bash
kops delete cluster --name=kubeapp.harshitch.xyz --state=s3://kopsbucket23021 --yes
```
Architecture
https://via.placeholder.com/600x400?text=K8s+Architecture+Diagram
