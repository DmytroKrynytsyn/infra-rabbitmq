# infra-rabbitmq

Helm chart + ArgoCD Application deploying RabbitMQ on k3s. Used as a request buffer between apps and LLM inference nodes (kbrain/kbrain2). GitOps only — no manual `kubectl apply`/`helm install` against the live cluster except the one-time ArgoCD bootstrap in the README.

## Code style

- Simple, minimal solutions. No abstractions, variables, or conditionals for cases that don't exist yet.
- All tunables (image tag, resources, `nodeSelector`, storage, ports, ingress host, etc.) live in `helm/rabbitmq/values.yaml` — templates stay generic.
- RabbitMQ runtime config goes through the `rabbitmq-config` ConfigMap (`rabbitmq.conf`), mounted into the deployment. The deployment has a `checksum/config` annotation so pods restart automatically when the ConfigMap changes.
- Single-node deployment, pinned via `nodeSelector` to nodes labeled `purpose=kappsdata`. All pods belonging to this project's infrastructure must use this nodeSelector — don't deploy any pod here without it.

## Verifying changes

- Validate templates render before committing: `helm template helm/rabbitmq` (or `helm lint helm/rabbitmq`).
- This repo is the source of truth for ArgoCD with `selfHeal` and `prune` enabled — a bad commit on `main` gets applied automatically. Double-check changes before pushing.
