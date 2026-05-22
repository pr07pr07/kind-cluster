# kind-cluster

> Kind cluster setup and configuration on Mac laptop

## Overview

This repository contains the configuration and scripts for setting up a local Kubernetes cluster using [Kind](https://kind.sigs.k8s.io/) (Kubernetes IN Docker) on macOS.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) (v26.0.0+)
- [Kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation) (v0.20+)
- [kubectl](https://kubernetes.io/docs/tasks/tools/) (v1.28+)
- macOS 14+ (Sonoma/Ventura)

## Quick Start

### 1. Create Cluster

```bash
# Create a basic cluster
kind create cluster --name my-cluster

# Create cluster with custom config
kind create cluster --name my-cluster --config kind-config.yaml
```

### 2. Verify Setup

```bash
# Check cluster info
kubectl cluster-info

# List nodes
kubectl get nodes

# List all pods
kubectl get pods -A
```

### 3. Delete Cluster

```bash
kind delete cluster --name my-cluster
```

## Cluster Configurations

### My Learning Cluster Config

This is my actual Kind cluster configuration (`kind-my-learning-cluster`):

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
```

### Default Config (kind-config.yaml)

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 80
        hostPort: 80
        protocol: TCP
      - containerPort: 443
        hostPort: 443
        protocol: TCP
```

### Multi-Node Config

```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
```

## My Current Clusters

| Cluster Name | Type | Purpose |
|-------------|------|---------|
| `my-learning-cluster` | Kind | Learning k8s |
| `msentraid-cluster` | Kind | Testing |
| `cegal-dev-aks` | AKS | Dev environment |
| `collabor8-dev-propel-noe-aks` | AKS | Collaboration dev |
| `collabor8-prod-mm-euw-aks` | AKS | Production EU |

## Useful Commands

```bash
# Get kubeconfig for a cluster
kind get kubeconfig --name my-learning-cluster

# Export kubeconfig to default location
kind export kubeconfig --name my-learning-cluster

# Load docker image into cluster
kind load docker-image my-image:latest --name my-cluster

# List all kind clusters
kind get clusters

# Describe node details
kubectl describe node kind-control-plane

# Switch between clusters
kubectl config use-context kind-my-learning-cluster
kubectl config use-context cegal-dev-aks
```

## Troubleshooting

### Docker daemon not running

```bash
# Start Docker Desktop or
open -a Docker
```

### Port conflicts (80/443)

Remove port mappings from `kind-config.yaml` or change to unused ports.

### Reset cluster

```bash
kind delete cluster --name my-learning-cluster
kind create cluster --name my-learning-cluster --config kind-config.yaml
```

## References

- [Kind Documentation](https://kind.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/)

---

**Author:** Prathap Dasari  
**Location:** Oslo, Norway  
**Company:** CEGAL AS