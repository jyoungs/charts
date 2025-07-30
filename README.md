# # Progressive Delivery Charts

A collection of Helm charts demonstrating advanced deployment strategies with Argo Rollouts and Linkerd service mesh.

## Charts

### 🟦 [helm-blue-green](./helm-blue-green/)
Blue-Green deployment with manual approval gates and automated analysis.

**Demo Features:**
- Manual approval required before promotion
- Pre and post-promotion analysis
- Linkerd metrics integration
- Safe rollback capabilities

### 🟡 [helm-canary](./helm-canary/)  
Canary deployment with progressive traffic shifting and SMI TrafficSplit.

**Demo Features:**
- Progressive traffic: 10% → 30% → 50% → 100%
- Automated analysis at each stage
- Manual approval before final promotion
- SMI TrafficSplit for Linkerd

## Prerequisites

- **Kubernetes cluster**
- **Argo Rollouts**: `kubectl create namespace argo-rollouts && kubectl apply -n argo-rollouts -f https://github.com/argoproj/argo-rollouts/releases/latest/download/install.yaml`
- **Linkerd**: Service mesh with Prometheus at `prometheus.linkerd.svc.cluster.local:9090`
- **kubectl-argo-rollouts**: CLI plugin for managing rollouts

## Quick Start

```bash
# Deploy blue-green rollout
helm install demo-bg helm-blue-green/

# Deploy canary rollout  
helm install demo-canary helm-canary/

# Watch progress
kubectl argo rollouts get rollout demo-bg --watch
kubectl argo rollouts get rollout demo-canary --watch

# Promote when ready
kubectl argo rollouts promote demo-bg
kubectl argo rollouts promote demo-canary
```

## Demo Script

1. **Show initial deployment**
2. **Trigger update**: `helm upgrade demo-bg helm-blue-green/ --set image.tag=0.2-plain-text`
3. **Watch automated analysis**: Success rate and latency checks
4. **Manual approval**: `kubectl argo rollouts promote demo-bg`
5. **Show traffic distribution**: Linkerd dashboard or `kubectl get trafficsplit`