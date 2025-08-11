# Kubernetes Installation Guide for Checkmate

This guide walks you through deploying Checkmate on your Kubernetes cluster using Helm.

## Prerequisites

- A running Kubernetes cluster
- Helm CLI installed and configured
- `kubectl` configured to access your cluster

## Steps

### 1. Clone the repo and navigate to the Helm chart

```bash
git clone https://github.com/bluewave-labs/checkmate.git
cd checkmate/charts/helm/checkmate
```

### 2. Update chart dependencies
```bash
helm dependency update
```

### 3. Customize values.yaml
Edit `values.yaml` to update:
- `client.ingress.main.hosts[0].host` and `server.ingress.main.hosts[0].host` with your domain names
- Secrets under the `secrets` section (`JWT_SECRET`, email credentials, API keys, etc.) — replace all `change_me` values
- Optionally disable bundled MongoDB or Redis by setting `mongodb.enabled` or `redis.enabled` to `false`

### 4. Deploy the Helm chart
```bash
helm install checkmate ./charts/helm/checkmate
```
This will deploy the client, server, MongoDB, and Redis components using their respective subcharts.

### 5. Verify the deployment
Check pods and services:
```bash
kubectl get pods
kubectl get svc
```

Once all pods are `Running` and `Ready`, you can access Checkmate via the configured ingress hosts.
