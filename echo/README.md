# Echo Helm Chart

A generic Helm chart for Kubernetes deployments with Istio VirtualService support.

## Description

This chart deploys a containerized application to Kubernetes with support for:
- Kubernetes Deployment with configurable replicas and rolling updates
- Service (ClusterIP, NodePort, or LoadBalancer)
- Istio VirtualService for traffic routing
- Horizontal Pod Autoscaler (HPA) for automatic scaling

## Prerequisites

- Kubernetes 1.19+
- Helm 3.0+
- Istio (if using VirtualService)

## Installation

```bash
helm install <release-name> . -f values.yaml
```

For development environment:
```bash
helm install <release-name> . -f values-dev.yml
```

## Values Files

### values.yaml
The default values file containing all configuration options with production-ready defaults.

### values-dev.yml
Development-specific overrides that customize the chart for development environments. This file overrides the default `values.yaml` with:

- **Namespace**: Sets deployment namespace to `dev` instead of `core`
- **Image**: Uses `nginx:latest` instead of `hashicorp/http-echo`
- **Container Port**: Configures port `80` instead of `5678`
- **Service/VirtualService**: These sections are commented out by default, allowing you to enable them as needed

When installing with `values-dev.yml`, Helm merges these values with the defaults from `values.yaml`, so any values not specified in `values-dev.yml` will use the defaults.

**Example values-dev.yml overrides:**
```yaml
deployment:
  namespace: "dev"
image:
  repository: nginx
  tag: "latest"
container:
  ports:
    - name: http
      containerPort: 80
      protocol: TCP
```

## Configuration

The chart supports extensive configuration through `values.yaml`. Key parameters include:

- **deployment**: Name, namespace, replicas, labels, and update strategy
- **image**: Repository, tag, pull policy, and pull secrets
- **container**: Ports, command, args, environment variables
- **resources**: CPU and memory requests/limits
- **service**: Type, port, annotations, labels
- **virtualService**: Hosts, gateways, HTTP routes (Istio)
- **autoscaling**: Min/max replicas, CPU/memory targets

See `values.yaml` for all available configuration options.

## Default Values

By default, the chart:
- Uses `hashicorp/http-echo` image
- Deploys 1 replica
- Creates a ClusterIP service on port 8080
- Enables Istio VirtualService with default hosts and gateways
- Disables HPA

## Example

```bash
helm install my-app . \
  --set deployment.name=my-app \
  --set image.repository=my-registry/my-app \
  --set image.tag=v1.0.0 \
  --set deployment.replicas=3
```

## Uninstallation

```bash
helm uninstall <release-name>
```

