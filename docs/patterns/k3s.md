# K3s Pattern

**Full Kubernetes cluster for cloud-native applications**

The k3s pattern deploys a complete Kubernetes cluster using K3s (lightweight Kubernetes) with management tools, load balancing, and ingress controllers pre-configured.

---

## Overview

```
┌──────────────────────────────┐
│  K3s Kubernetes Cluster      │
│  ┌────────────────────────┐  │
│  │  Management Node       │  │
│  │  (kubectl, helm, k9s)  │  │
│  └────────────────────────┘  │
│  ┌────────────────────────┐  │
│  │  Control + Workers     │  │
│  │  (MetalLB + Ingress)   │  │
│  └────────────────────────┘  │
└──────────────────────────────┘
```

**Perfect For:**

- Cloud-native applications
- Microservices architectures
- Enterprise deployments
- Production SaaS at scale

---

## Quick Start

```bash
tfgrid-compose up my-cluster --pattern=k3s
```

**Deploy time:** 10-15 minutes  
**Cost:** $100-500/month

---

## Features

- ☸️ **Full Kubernetes cluster** - Production-ready K3s deployment
- ⚖️ **MetalLB load balancer** - Built-in load balancing for services
- 🌐 **Nginx Ingress** - HTTP/HTTPS routing to your applications
- 📈 **Auto-scaling** - Horizontal pod autoscaling ready
- 🛡️ **HA control plane** - High availability for production
- 🔧 **Management tools** - kubectl, helm, k9s pre-installed
- 📦 **Persistent storage** - StatefulSets and persistent volumes supported

---

## Example Deployment

Deploy a Kubernetes cluster:

```bash
$ tfgrid-compose up my-cluster --pattern=k3s

✨ Kubernetes ready in 10 minutes!
```

Access your cluster:

```bash
$ tfgrid-compose ssh my-cluster
# Now you have kubectl, helm, and k9s available
kubectl get nodes
```

---

## Architecture

### Management Node
- SSH access point
- kubectl configured and ready
- helm package manager
- k9s TUI for cluster management
- Direct access to cluster API

### Control Plane Node(s)
- K3s server (control plane)
- etcd datastore
- API server
- Scheduler and controller manager

### Worker Nodes
- K3s agent
- Container runtime
- Pod networking
- Storage provisioning

### Network Components
- **MetalLB** - Layer 2/BGP load balancer
- **Nginx Ingress** - HTTP/HTTPS routing
- **Calico/Flannel** - Pod networking
- **WireGuard** - Secure node communication

---

## Configuration

Example `tfgrid-compose.yaml` for k3s pattern:

```yaml
name: my-cluster
pattern: k3s

cluster:
  control_nodes: 1  # or 3 for HA
  worker_nodes: 3
  
nodes:
  control:
    cpu: 4
    memory: 8192
    storage: 100
  
  worker:
    cpu: 4
    memory: 16384
    storage: 200

features:
  metallb: true
  ingress: true
  monitoring: true
```

---

## Use Cases

### Microservices Architecture
Deploy and orchestrate microservices:

```bash
tfgrid-compose up my-services --pattern=k3s
# Then deploy your microservices via kubectl/helm
```

### Cloud-Native SaaS
Run production SaaS at scale:

```bash
tfgrid-compose up prod-saas --pattern=k3s
```

### Multi-Tenant Applications
Deploy applications with tenant isolation:

```bash
tfgrid-compose up multi-tenant --pattern=k3s
```

### CI/CD Platforms
Run Jenkins, GitLab, or other CI/CD tools:

```bash
tfgrid-compose up cicd-cluster --pattern=k3s
```

---

## Kubernetes Features

### Deployments
Standard Kubernetes deployments work out of the box:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app
spec:
  replicas: 3
  template:
    spec:
      containers:
      - name: app
        image: myapp:latest
```

### Services & Ingress
Expose applications with LoadBalancer or Ingress:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-app
spec:
  type: LoadBalancer  # MetalLB provides the IP
  ports:
  - port: 80
    targetPort: 8080
```

### Persistent Storage
Use persistent volumes for stateful applications:

```yaml
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: data
spec:
  accessModes:
  - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

---

## Management Tools

### kubectl
Standard Kubernetes CLI - pre-configured and ready:

```bash
kubectl get pods --all-namespaces
kubectl apply -f deployment.yaml
kubectl logs my-pod
```

### helm
Kubernetes package manager:

```bash
helm repo add stable https://charts.helm.sh/stable
helm install my-app stable/nginx
helm list
```

### k9s
Terminal UI for Kubernetes:

```bash
k9s  # Interactive cluster management
```

---

## High Availability

For production deployments, use 3 control plane nodes:

```yaml
cluster:
  control_nodes: 3  # HA configuration
  worker_nodes: 5
```

This provides:
- **Control plane redundancy** - No single point of failure
- **Automatic failover** - If one control node fails, others continue
- **Load distribution** - API requests distributed across nodes

---

## Monitoring & Observability

The k3s pattern can be configured with monitoring:

```yaml
features:
  monitoring: true  # Deploys Prometheus + Grafana
  logging: true     # Centralized logging
```

---

## Scaling

### Manual Scaling
Scale deployments manually:

```bash
kubectl scale deployment my-app --replicas=10
```

### Horizontal Pod Autoscaling
Configure automatic scaling based on metrics:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: my-app
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: my-app
  minReplicas: 3
  maxReplicas: 10
  metrics:
  - type: Resource
    resource:
      name: cpu
      target:
        type: Utilization
        averageUtilization: 70
```

---

## Full Documentation

For complete implementation details, see the [k3s pattern source](https://github.com/tfgrid-studio/tfgrid-compose/tree/main/patterns/k3s).

---

## Next Steps

- [Deploy your first Kubernetes cluster](../getting-started/quickstart.md)
- [Learn about Gateway pattern](gateway.md) for simpler web apps
- [Explore Single-VM pattern](single-vm.md) for development environments
