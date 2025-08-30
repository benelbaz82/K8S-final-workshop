# WordPress + DB on Kubernetes (Helm Chart)

This repo contains a minimal Helm chart that deploys **WordPress** with a **MySQL database** on Kubernetes.  
The chart simply packages the existing Kubernetes manifests into a Helm release, without complex templating.

## Install / Upgrade

```bash
helm upgrade --install myapp ./ -n default

### Notes

Monitoring is provided by the centralized kube-prometheus-stack.

A Grafana panel was created to track uptime of both WordPress and DB containers.

All manifests (WordPress Deployment/Service and DB StatefulSet/Service) are included under /templates.


