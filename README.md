# Kubernetes Final Workshop – WordPress + MySQL + Monitoring

This repository contains a Helm chart for deploying a WordPress + MySQL stack on Kubernetes.
It also includes instructions for installing NGINX Ingress Controller and kube-prometheus-stack (Prometheus + Grafana) for monitoring.

## Requirements

- AWS EC2 instance with Minikube installed and running

- kubectl installed and configured

- helm installed

- Docker installed 

## Setup Instructions
Clone the Repository
```
git clone https://github.com/benelbaz82/K8S-final-workshop.git
cd K8S-final-workshop/myapp-helm
```
Install NGINX Ingress Controller
```
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update
helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace
```

Verify installation:
```
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx
```
### Deploy WordPress + MySQL

Deploy the Helm chart from this repo:
```
helm upgrade --install myapp .
```

### Check resources:
```
kubectl get pods
kubectl get svc
kubectl get pvc
```
### Access WordPress

Find Minikube IP:
```
minikube ip
```

Add it to /etc/hosts:
```
<MINIKUBE_IP>   wordpress.local
```

Then open in browser:

http://wordpress.local

### Install Monitoring (Prometheus + Grafana)
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade --install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring --create-namespace
```

Check pods:
```
kubectl get pods -n monitoring
```
### Access Grafana

Forward Grafana service:
```
kubectl port-forward svc/monitoring-grafana -n monitoring 3000:80
```

Open in browser:

http://localhost:3000

```
kubectl get secret monitoring-grafana -n monitoring -o jsonpath="{.data.admin-password}" | base64 --decode
```
### Create Grafana Panel

Go to Grafana → Explore

Run query:

kube_pod_container_status_running


Save it to a dashboard panel → shows container uptime.
