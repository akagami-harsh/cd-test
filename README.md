# GitOps Testing Deployments

This repository contains testing deployments for GitOps using **Fleet** and **Rancher**.

## Structure

```
.
├── fleet.yaml              # Fleet bundle configuration
├── apps/
│   ├── nginx-demo/         # Simple nginx deployment
│   ├── redis-demo/         # Redis with ConfigMap
│   └── httpbin/            # HTTPBin API testing service
├── base/                   # Shared base configurations
│   └── namespace.yaml      # Common namespace
└── overlays/
    ├── dev/                # Development environment
    └── prod/               # Production environment (optional)
```

## Quick Start

### Prerequisites
- Rancher with Fleet enabled
- kubectl configured to your cluster
- Git repository accessible from your Rancher instance

### Usage with Fleet

1. **Register this repository in Rancher Fleet:**
   - Go to Rancher → Continuous Delivery → Git Repos
   - Add this repository URL
   - Fleet will automatically discover and deploy the bundles

2. **Manual testing:**
   ```bash
   kubectl apply -k overlays/dev/
   ```

## Applications

| App | Description | Port |
|-----|-------------|------|
| nginx-demo | Simple nginx web server | 80 |
| redis-demo | Redis cache with custom config | 6379 |
| httpbin | HTTP Request & Response Service | 80 |

## Fleet Configuration

The `fleet.yaml` defines targeting rules and deployment options. By default, deployments target clusters with the label `env: dev`.

## Testing Connectivity

```bash
# Port forward nginx
kubectl port-forward -n gitops-test svc/nginx-demo 8080:80

# Port forward httpbin
kubectl port-forward -n gitops-test svc/httpbin 8081:80

# Test redis
kubectl exec -n gitops-test -it deploy/redis-demo -- redis-cli ping
```
