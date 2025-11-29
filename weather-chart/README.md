# weather-chart

A small Helm chart to deploy the weather app to Kubernetes.

This README documents the chart usage, configurable values in `values.yaml`, and some examples to customize the installation.

## Chart Details
- Chart name: `weather-chart` (see `Chart.yaml`)
- App version: defined in `Chart.yaml` as `appVersion`

## Values
See `values.yaml` for the full list of configurable options. Important values include:

- `replicaCount` - Number of replicas to create when autoscaling is disabled.
- `image.repository` - Container image repository (default `nginxdemos/hello`).
- `image.pullPolicy` - Image pull policy (IfNotPresent/Always).
- `image.tag` - Image tag/version.
- `service.type` - Kubernetes Service type (ClusterIP, NodePort, LoadBalancer).
- `service.port` - Service port (default 80).
- `ingress.enabled` - Whether to create an Ingress.
- `ingress.className` - Ingress class (e.g., "nginx").
- `ingress.hosts` - Path and host configuration.
- `resources` - Container CPU/memory settings (requests/limits).
- `autoscaling.enabled` - Enable Horizontal Pod Autoscaler (HPA).
- `autoscaling.minReplicas`, `autoscaling.maxReplicas`, `autoscaling.targetCPUUtilizationPercentage` - HPA settings.

## Example installation

Install with Helm using default values:

```bash
helm install weather-app ./weather-chart --namespace weather-prod --create-namespace
```

Install with custom values (e.g., custom image tag and disable ingress):

```bash
helm install weather-app ./weather-chart \
  --namespace weather-prod --create-namespace \
  --set image.repository=myrepo/weather-app \
  --set image.tag=v1.2.3 \
  --set ingress.enabled=false
```

Upgrade example:

```bash
helm upgrade weather-app ./weather-chart --namespace weather-prod --set image.tag=v1.2.4
```

Uninstall:

```bash
helm uninstall weather-app --namespace weather-prod
```

## Post-install notes
This chart contains a `NOTES.txt` template that prints helpful resources and access tips after install. See it under `templates/NOTES.txt`.

## Development
- Use `helm lint ./weather-chart` and `helm template ./weather-chart` during development.
- Template functions live in `templates/_helpers.tpl`.

## Contributions
Please open PRs and ensure tests (lint + template) pass.

---

Happy charting!
