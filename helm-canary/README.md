# Helm Canary Chart

A Helm chart for demonstrating Canary deployments with progressive traffic shifting using Argo Rollouts and Linkerd service mesh.

## Features

- **Canary deployment strategy** with progressive traffic shifting
- **SMI TrafficSplit integration** for Linkerd-based traffic management
- **Automated analysis** at each traffic shifting stage
- **Configurable traffic progression**: 10% → 30% → 50% → Manual Approval → 100%
- **Manual approval gate** before final promotion

## Prerequisites

- Kubernetes cluster with Argo Rollouts installed
- Linkerd service mesh installed with SMI extension
- Prometheus accessible at `prometheus.linkerd.svc.cluster.local:9090`

## Usage

```bash
# Install the chart
helm install my-canary helm-canary/

# Watch the rollout progress
kubectl argo rollouts get rollout my-canary --watch

# Promote to next stage or approve final promotion
kubectl argo rollouts promote my-canary
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Container image repository | `nginxdemos/hello` |
| `image.tag` | Container image tag | `plain-text` |
| `linkerd.inject` | Linkerd proxy injection | `enabled` |
| `canary.steps.setWeight1` | First traffic weight | `10` |
| `canary.steps.pause1Duration` | First pause duration | `2m` |
| `canary.steps.setWeight2` | Second traffic weight | `30` |
| `canary.steps.pause2Duration` | Second pause duration | `5m` |
| `canary.steps.setWeight3` | Third traffic weight | `50` |

## Demo Flow

1. **Deploy**: Canary version deployed (0% traffic)
2. **10% Traffic**: Automated shift + 2min pause + analysis
3. **30% Traffic**: Automated shift + 5min pause + analysis
4. **50% Traffic**: Automated shift + manual approval required
5. **100% Traffic**: Final promotion after approval
6. **Complete**: Old version scaled down

## Traffic Management

The chart uses SMI TrafficSplit to manage traffic distribution:
- **Root Service**: `{release-name}` (receives all traffic)
- **Stable Service**: `{release-name}-stable` (current version)
- **Canary Service**: `{release-name}-canary` (new version)
- **TrafficSplit**: Automatically managed by Argo Rollouts
