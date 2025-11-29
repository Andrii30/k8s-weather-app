# k8s-weather-app

A small demo Kubernetes application and Helm chart to deploy a simple weather app.

This repository contains:
- `argo-app.yaml` - ArgoCD Application manifest for deploying the chart through ArgoCD
- `weather-chart/` - a Helm chart for deploying the weather app

## Prerequisites
- Kubernetes cluster (Minikube / Kind / GKE / EKS / AKS)
- Helm 3 (`helm`) installed and configured
- kubectl configured for your cluster
- (Optional) ArgoCD controller to deploy using GitOps

## Quick Install (using Helm)
1. Add/Package or use the chart path directly:

```bash
# In the repo root
helm lint ./weather-chart
helm template weather-chart ./weather-chart

# Install
helm install weather-app ./weather-chart --namespace weather-prod --create-namespace
```

2. Verify resources:

```bash
kubectl get all -n weather-prod
kubectl get ingress -n weather-prod
```

3. Access the app:
- If using Ingress, use the host configured (`weather.local` in defaults) and configure your /etc/hosts or a DNS entry to point to your cluster.
- Or run a port-forward to the Service:

```bash
kubectl -n weather-prod port-forward svc/weather-app 8080:80
# Now open http://localhost:8080
```

## Install with ArgoCD
1. Apply ArgoCD app manifest:

```bash
kubectl apply -f argo-app.yaml
```

2. In ArgoCD UI, sync the `weather-app` application or wait for automated sync.

## Configuration
The chart default values are in `weather-chart/values.yaml`. You can override values during `helm install` or with a values file:

```bash
helm install weather-app ./weather-chart \
  --namespace weather-prod \
  --create-namespace \
  --set image.repository=myrepo/weather-app,image.tag=1.0.1
```

## Notes
- The chart supports Autoscaling via `autoscaling.enabled` and will use `replicaCount` only when autoscaling is disabled.
- To change ingress settings, modify `ingress.enabled`, `ingress.className`, and `ingress.hosts` in `values.yaml` or set them at install time.

## Contributing
1. Fork this repo and create a branch for your changes.
2. Run `helm lint` and `helm template` to verify the chart.
3. Open a pull request and describe the changes.

## License
MIT
