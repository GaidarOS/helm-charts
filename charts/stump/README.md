# Stump Chart

A Helm chart for Stump, a free and open source comics, manga and digital book server with OPDS support

Source code can be found here:
- https://github.com/stumpapp/stump

This is a **community maintained** chart. This chart installs stump, a free and open source comics, manga and digital book server with OPDS support.


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
