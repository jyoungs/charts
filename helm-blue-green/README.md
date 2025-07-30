# Helm Blue-Green Chart

A Helm chart for demonstrating Blue-Green deployments with Argo Rollouts and Linkerd service mesh.

## Features

- **Blue-Green deployment strategy** with manual approval gates
- **Linkerd service mesh integration** for observability and traffic management
- **Automated analysis** using Prometheus metrics before and after promotion
- **Manual approval required** for production promotion
- **Configurable scale-down delay** for safe rollbacks

## Prerequisites

- Kubernetes cluster with Argo Rollouts installed
- Linkerd service mesh installed
- Prometheus accessible at `prometheus.linkerd.svc.cluster.local:9090`

## Usage

```bash
# Install the chart
helm install my-blue-green helm-blue-green/

# Watch the rollout progress
kubectl argo rollouts get rollout my-blue-green --watch

# Promote after manual review
kubectl argo rollouts promote my-blue-green
```

## Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.repository` | Container image repository | `nginxdemos/hello` |
| `image.tag` | Container image tag | `plain-text` |
| `linkerd.inject` | Linkerd proxy injection | `enabled` |
| `blueGreen.scaleDownDelaySeconds` | Delay before scaling down old version | `30` |

## Demo Flow

1. **Deploy**: New version deployed to preview service
2. **Analysis**: Automated success rate and latency analysis
3. **Manual Approval**: Review required before promotion
4. **Promote**: Traffic switched to new version
5. **Post-Analysis**: Verification of promoted version
6. **Scale Down**: Old version removed after delay
