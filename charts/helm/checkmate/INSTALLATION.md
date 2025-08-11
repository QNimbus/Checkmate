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

#### Required Changes:
- Secrets under the `secrets` section (`JWT_SECRET`, `REFRESH_TOKEN_SECRET`, email credentials, API keys, etc.) — replace all `change_me` values

#### Optional Changes:
- **For ingress setup**: Set `client.ingress.main.enabled: true` and `server.ingress.main.enabled: true`, then configure the hosts:
  ```yaml
  client:
    ingress:
      main:
        enabled: true
        hosts:
          - host: your-client-domain.com
            paths:
              - path: /
                pathType: Prefix
  
  server:
    ingress:
      main:
        enabled: true
        hosts:
          - host: your-server-domain.com
            paths:
              - path: /
                pathType: Prefix
  ```
- **For external ingress**: Keep `ingress.main.enabled: false` (default) and configure your own ingress controller
- **For external databases**: Disable bundled MongoDB or Redis by setting `mongodb.enabled` or `redis.enabled` to `false`

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

### 6. Access the application

#### With Ingress Enabled:
Once all pods are `Running` and `Ready`, you can access Checkmate via the configured ingress hosts.

#### With Ingress Disabled:
You can access the services using port-forwarding:
```bash
# Access client
kubectl port-forward service/checkmate-client 8080:80

# Access server (in another terminal)
kubectl port-forward service/checkmate-server 8081:52345
```

Then visit:
- Client: http://localhost:8080
- Server: http://localhost:8081
