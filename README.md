# AWS Nginx Ingress

## Overview
This repository contains configurations for deploying Nginx Ingress on AWS, using Kubernetes. The setup enables ingress traffic management and load balancing for applications running in an AWS-based Kubernetes cluster.

## Prerequisites
Before setting up the Nginx Ingress, ensure you have the following:
- AWS account
- Kubernetes cluster (EKS)
- kubectl installed and configured
- Helm installed
- AWS CLI configured

## Installation

### 1. Apply Terraform File

```sh
terraform apply
```

### 2. Deploy Nginx Ingress Controller
You can install the Nginx Ingress Controller using Helm:
```sh
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm install ingress-nginx ingress-nginx/ingress-nginx \
    --namespace ingress-nginx --create-namespace \
    --set controller.replicaCount=2 \
    --set controller.service.type=LoadBalancer
```

### 3. Verify Deployment
Check if the Ingress Controller pods are running:
```sh
kubectl get pods -n ingress-nginx
```
Get the LoadBalancer service details:
```sh
kubectl get svc -n ingress-nginx
```

### 4. Configure Ingress Resource
Apply the Kubernetes resource:
```sh
kubectl apply -f /K8s
```

### 5. Testing
Check if the ingress resource is created:
```sh
kubectl get ingress
```

Check if the ingress resource have not error:
```sh
kubectl describe ingress
```

### Access your application using the external LoadBalancer IP or domain name.
If a user accesses `<external-ip>/app1/test`, the ingress will rewrite the path to `/test` and forward the request to `flask-service`. Similarly, if a user accesses `<external-ip>/app2/test`, the ingress will rewrite the path to `/test` and forward the request to `flask-service-test`.


## Cleanup
To remove the Nginx Ingress setup:
```sh
helm uninstall nginx-ingress
kubectl delete -f /K8s
terraform destroy
```
