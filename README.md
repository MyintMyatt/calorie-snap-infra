This is the standard approach if you plan to use Kustomize (built into kubectl) or GitOps tools like ArgoCD. It separates your base configurations from environment-specific overrides, preventing code duplication.textk8s-infrastructure/
```
├── base/
|   ├── aws-eks-cluster/
|   |   ├── cluster-config.yaml
│   ├── kafka/
│   │   ├── kustomization.yaml
│   │   ├── statefulset.yaml
│   │   ├── service.yaml
│   │   └── configmap.yaml
│   ├── rabbitmq/
│   │   ├── kustomization.yaml
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   ├── redis/
│   │   ├── kustomization.yaml
│   │   ├── deployment.yaml
│   │   └── service.yaml
│   └── microservices/
│       ├── auth-service/
│       │   ├── deployment.yaml
│       │   └── service.yaml
│       └── payment-service/
│           ├── deployment.yaml
│           └── service.yaml
└── overlays/
    ├── development/
    │   ├── kustomization.yaml
    │   ├── kafka-replica-patch.yaml
    │   └── redis-config-patch.yaml
    └── production/
        ├── kustomization.yaml
        ├── kafka-scale-patch.yaml
        └── secrets.encrypted.yaml
```


Why this works:
`base/:` Holds the fundamental manifests that don't change often (e.g., service definitions, port mappings).
`overlays/:` Contains environment-specific tweaks. For example, your development overlay might run a single Redis pod, while your production overlay scales it to a highly available cluster


## RabbitMQ
```bash
kubectl apply -f base/rabbitmq/
```
- If u run on local machine and also your backend services (go, spring boot) are not in same k8s cluster,you do this port forward for rabbitmq to local access from vscode or intellij for testing
```bash
kubectl port-forward -n rabbitmq-messaging  svc/rabbitmq-service 5672:5672
```