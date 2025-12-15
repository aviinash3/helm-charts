# Demo App Helm Chart

A production-grade Helm chart for deploying the demo application on Kubernetes.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- kubectl configured to access your cluster

## Installation

### Basic Installation

```bash
helm install demo-app . --namespace demo-app --create-namespace
```

### Installation with Custom Values

```bash
helm install demo-app . -f custom-values.yaml --namespace demo-app --create-namespace
```

### Upgrade

```bash
helm upgrade demo-app . --namespace demo-app
```

### Uninstall

```bash
helm uninstall demo-app --namespace demo-app
```

## Configuration

The following table lists the configurable parameters and their default values:

### Image Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `image.registry` | Container image registry | `docker.io` |
| `image.repository` | Container image repository | `nginx` |
| `image.tag` | Container image tag | `1.25-alpine` |
| `image.pullPolicy` | Image pull policy | `IfNotPresent` |
| `image.pullSecrets` | Image pull secrets | `[{name: demoappsecret}]` |

### Deployment Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `deployment.replicas` | Number of replicas | `2` |
| `deployment.strategy.type` | Deployment strategy | `RollingUpdate` |
| `deployment.revisionHistoryLimit` | Number of old replicas to retain | `10` |

### Resource Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `resources.requests.cpu` | CPU request | `100m` |
| `resources.requests.memory` | Memory request | `128Mi` |
| `resources.limits.cpu` | CPU limit | `500m` |
| `resources.limits.memory` | Memory limit | `512Mi` |

### Health Checks

| Parameter | Description | Default |
|-----------|-------------|---------|
| `healthChecks.enabled` | Enable health checks | `true` |
| `healthChecks.livenessProbe` | Liveness probe configuration | See values.yaml |
| `healthChecks.readinessProbe` | Readiness probe configuration | See values.yaml |
| `healthChecks.startupProbe` | Startup probe configuration | See values.yaml |

### Security

| Parameter | Description | Default |
|-----------|-------------|---------|
| `securityContext.enabled` | Enable security context | `true` |
| `securityContext.runAsNonRoot` | Run as non-root user | `true` |
| `securityContext.runAsUser` | User ID | `101` |
| `podSecurityContext.enabled` | Enable pod security context | `true` |

### Service Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `service.enabled` | Enable service | `true` |
| `service.type` | Service type | `ClusterIP` |
| `service.port` | Service port | `80` |

### Ingress Configuration

| Parameter | Description | Default |
|-----------|-------------|---------|
| `ingress.enabled` | Enable ingress | `true` |
| `ingress.className` | Ingress class name | `azure/application-gateway` |
| `ingress.hosts` | Ingress hosts configuration | See values.yaml |
| `ingress.tls` | TLS configuration | `[]` |

### Autoscaling

| Parameter | Description | Default |
|-----------|-------------|---------|
| `autoscaling.enabled` | Enable HPA | `true` |
| `autoscaling.minReplicas` | Minimum replicas | `2` |
| `autoscaling.maxReplicas` | Maximum replicas | `10` |
| `autoscaling.targetCPUUtilizationPercentage` | Target CPU utilization | `70` |
| `autoscaling.targetMemoryUtilizationPercentage` | Target memory utilization | `80` |

### Pod Disruption Budget

| Parameter | Description | Default |
|-----------|-------------|---------|
| `podDisruptionBudget.enabled` | Enable PDB | `true` |
| `podDisruptionBudget.minAvailable` | Minimum available pods | `1` |

## Production Best Practices

This chart includes several production-grade features:

1. **High Availability**: Multiple replicas with pod anti-affinity
2. **Health Checks**: Liveness, readiness, and startup probes
3. **Security**: Non-root user, read-only root filesystem, dropped capabilities
4. **Resource Management**: CPU and memory requests/limits
5. **Autoscaling**: Horizontal Pod Autoscaler based on CPU and memory
6. **Disaster Recovery**: Pod Disruption Budget for availability
7. **Network Security**: Optional NetworkPolicy for traffic control
8. **Rolling Updates**: Zero-downtime deployments
9. **Service Account**: Dedicated service account with least privilege
10. **ConfigMaps**: Support for configuration management

## Examples

### Deploy with Custom Image

```yaml
image:
  repository: myregistry/myapp
  tag: v1.0.0
```

### Deploy with Custom Resources

```yaml
resources:
  requests:
    cpu: 200m
    memory: 256Mi
  limits:
    cpu: 1000m
    memory: 1Gi
```

### Deploy with Custom Ingress

```yaml
ingress:
  enabled: true
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: myapp-tls
      hosts:
        - myapp.example.com
```

### Enable Network Policy

```yaml
networkPolicy:
  enabled: true
  policyTypes:
    - Ingress
    - Egress
```

## Troubleshooting

### Check Pod Status

```bash
kubectl get pods -n demo-app
```

### Check Logs

```bash
kubectl logs -n demo-app -l app.kubernetes.io/name=demo-app
```

### Describe Deployment

```bash
kubectl describe deployment -n demo-app demo-app
```

### Check HPA Status

```bash
kubectl get hpa -n demo-app
```

## Support

For issues and questions, please open an issue in the repository.

