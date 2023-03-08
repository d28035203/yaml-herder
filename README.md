# YAML Herder

Kubernetes manifests for a small demo API: namespace, Deployment, Service, ConfigMap, HPA, and a restrictive NetworkPolicy. Bundled with Kustomize.

## Apply

```bash
kubectl apply -k k8s/
kubectl -n yaml-herder get deploy,svc,hpa,networkpolicy
```

## Pieces

| File | Role |
|------|------|
| `namespace.yaml` | Isolates resources |
| `deployment.yaml` | 2 replicas of `hashicorp/http-echo`, readiness probe, resource requests |
| `service.yaml` | ClusterIP on port 80 → 5678 |
| `configmap.yaml` | `APP_ENV` / `LOG_LEVEL` |
| `hpa.yaml` | CPU target 70%, 2–6 replicas (needs metrics-server) |
| `networkpolicy.yaml` | Ingress only from pods labeled `role=frontend` |

## Notes

- Readiness is not liveness — probe success means traffic can flow, not that the process never restarts.
- HPA is a no-op without metrics-server.
- Swap the echo image for your real app image and wire secrets via SealedSecrets/SOPS outside this repo.

## License

MIT
