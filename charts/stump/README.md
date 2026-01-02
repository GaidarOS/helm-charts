# Stump Chart

A Helm chart for Stump, a free and open source comics, manga and digital book server with OPDS support

Source code can be found here:
- https://github.com/stumpapp/stump

This is a **community maintained** chart. This chart installs stump, a free and open source comics, manga and digital book server with OPDS support.

## Steps to Use a Helm Chart

### 1. Add a Helm Repository

Helm repositories contain collections of charts. You can add an existing repository using the following command:

```bash
helm repo add stump https://stump.github.io/stump-helm
```

### 2. Install the Helm Chart

To install a chart, use the following command:

```bash
helm install my-stump stump/stump
```

### 3. View the Installation

You can check the status of the release using:

```bash
helm status my-stump
```

## Customizing the Chart

Helm charts come with default values, but you can customize them by using the --set flag or by providing a custom values.yaml file.

### 1. Using --set to Override Values
```bash
helm install my-stump stump/stump --set key1=value1,key2=value2
```

### 2. Using a values.yaml File
You can create a custom values.yaml file and pass it to the install command:

```bash
helm install my-stump stump/stump -f values.yaml
```

## Ingress configuration

Standard Ingress implementation

### SSL Termination at Ingress Controller

```yaml
ingress:
  enabled: true
  className: nginx
  annotations:
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/backend-protocol: "HTTP"
  hosts:
    - host: stump.example.com
      paths:
        - path: /
          pathType: ImplementationSpecific
  tls:
    - hosts:
      - stump.example.com
      secretName: stump-or-wildcard-tls
```

### Gateway API HTTPRoute

The Gateway API provides a modern, extensible way to configure ingress traffic routing. This chart supports HTTPRoute resources as an alternative to traditional Ingress.

> **Note:**
> Gateway API support is **EXPERIMENTAL**. Support depends on your Gateway controller implementation. Refer to [Gateway API implementations](https://gateway-api.sigs.k8s.io/implementations/) for controller-specific details.

```yaml
global:
  domain: stump.example.com

server:
  httproute:
    enabled: true
    parentRefs:
      - name: example-gateway
        namespace: gateway-system
        sectionName: https
```

## Values

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| httpRoute | object | `{"annotations":{},"enabled":false,"hostnames":[],"parentRefs":[],"rules":[{"matches":[{"path":{"type":"PathPrefix","value":"/"}}]}]}` | HTTPRoute configuration for Gateway API. See: https://gateway-api.sigs.k8s.io/ |
| httpRoute.hostnames | list | `[]` | Hostnames to match for this HTTPRoute |
| httpRoute.parentRefs | list | `[]` | Gateway references to attach this HTTPRoute to |
| httpRoute.rules | list | `[{"matches":[{"path":{"type":"PathPrefix","value":"/"}}]}]` | Rules for routing traffic |
