# Kubernetes Playground Environment

This repository contains a development/testing Kubernetes environment with common services and monitoring setup.

## Components

- **Monitoring Stack**: Prometheus Operator with AlertManager
- **Data Services**: Redis
- **Sample Application**: Basic web application with Prometheus metrics
- **Ingress**: NGINX Ingress Controller with TLS support

## Directory Structure

```
.
├── applications/     # Application workloads
├── data-services/    # Stateful services (Redis, etc.)
├── ingress/         # Ingress configurations
├── monitoring/      # Prometheus and monitoring configs
└── namespaces/      # Namespace definitions
```

## Prerequisites

- Kubernetes cluster (1.20+)
- Helm 3.x
- kubectl configured to access your cluster
- NGINX Ingress Controller installed
- cert-manager (optional, for TLS)

## Installation

1. Create namespaces first:
   ```bash
   kubectl apply -f namespaces/namespaces.yaml
   ```

2. Install Prometheus Operator:
   ```bash
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring
   ```

3. Deploy components:
   ```bash
   kubectl apply -f data-services/
   kubectl apply -f applications/
   kubectl apply -f monitoring/
   kubectl apply -f ingress/
   ```

## Access Services

- Sample Application: https://playground.example.com/app
- Prometheus: https://monitoring.example.com


## Local Setup with Minikube

### Prerequisites
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- Same prerequisites as above (Helm, kubectl)

### Setup Instructions

1. Start your local cluster:
   ```bash
   minikube start --cpus 4 --memory 8192
   ```

2. Enable ingress:
   ```bash
   minikube addons enable ingress
   ```

3. Follow the same installation steps as above:
   ```bash
   # Create namespaces
   kubectl apply -f namespaces/namespaces.yaml

   # Install Prometheus Operator
   helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
   helm repo update
   helm install prometheus prometheus-community/kube-prometheus-stack -n monitoring

   # Deploy components
   kubectl apply -f data-services/
   kubectl apply -f applications/
   kubectl apply -f monitoring/
   kubectl apply -f ingress/
   ```

4. Access services:
   ```bash
   # Get Minikube IP
   minikube ip

   # Add to /etc/hosts:
   # <minikube-ip> playground.example.com monitoring.example.com
   ```

The environment should now work exactly the same as it would in any other Kubernetes cluster!

## Namespace Organization

- **monitoring**: Observability tools (Prometheus, AlertManager)
- **data-services**: Stateful services (Redis)
- **applications**: Application workloads

## Configuration

Update the following configurations according to your environment:

1. Update ingress hostnames in `ingress/ingress.yaml`
2. Adjust resource limits and requests in deployments if needed
3. Configure storage classes according to your cluster setup
4. Update monitoring endpoints and alerts as required

## Security Notes

- Basic authentication is enabled for monitoring endpoints
- TLS is configured for all ingress endpoints
- Namespace isolation is implemented
- Resource limits are set for all workloads

## Contributing

Feel free to submit issues and enhancement requests!