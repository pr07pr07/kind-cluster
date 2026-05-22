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

## Useful Commands

```bash
# Load an image into the cluster
kind load docker-image my-image:latest --name my-cluster

# Get cluster kubeconfig
kind get kubeconfig --name my-cluster

# Export kubeconfig
kind export kubeconfig --name my-cluster --kubeconfig ~/.kube/config

# List all clusters
kind get clusters

# Describe node details
kubectl describe node kind-control-plane
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
kind delete cluster --name my-cluster
kind create cluster --name my-cluster --config kind-config.yaml
```

## References

- [Kind Documentation](https://kind.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Docker Desktop for Mac](https://docs.docker.com/desktop/mac/)

---

**Author:** Prathap Dasari  
**Location:** Oslo, Norway  
**Company:** CEGAL AS