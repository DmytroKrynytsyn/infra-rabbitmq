# infra-rabbitmq

RabbitMQ deployed on k3s via ArgoCD GitOps. Used as a request buffer between apps and LLM inference nodes.

## Bootstrap

```bash
kubectl apply -f argocd/application.yaml
```

## Access

Management UI: https://rabbitmq.local (add to /etc/hosts → kserver IP)

## AMQP endpoint (cluster-internal)
amqp://guest:guest@rabbitmq.rabbitmq.svc.cluster.local:5672/

## Pattern

Apps publish LLM requests to a request queue and consume responses from a reply queue,
decoupling request rate from inference throughput on kbrain/kbrain2.