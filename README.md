# Repositorio GitOps de SA — Práctica 9

Este repositorio público es la única fuente de verdad de aplicaciones; ArgoCD realiza toda reconciliación. La raíz `sa-p9-apps` implementa app-of-apps y reconstruye operadores y plataforma sin pasos manuales intermedios.

## Estructura

- `argocd/apps/`: AppProject y aplicaciones hijas para Rollouts, Kyverno, ingress-nginx, External Secrets, Velero y `sa-platform-prod`.
- `charts/`: un chart por microservicio o tarea, más el chart de PostgreSQL/RabbitMQ, con perfiles `dev` y `prod`.
- `manifests/prod/`: políticas de red y External Secrets respaldados por Google Secret Manager.
- `manifests/dr/`: schedule horario de Velero con retención de siete días.
- `policies/`: cuatro políticas Kyverno en modo Enforce.

Terraform instala únicamente ArgoCD y la aplicación raíz `sa-p9-apps`. Esa raíz instala el resto desde este repositorio. Workload Identity permite que Velero y External Secrets accedan solo a sus recursos GCP, sin llaves JSON dentro del clúster.

Las versiones se modifican exclusivamente en `charts/*/values-prod.yaml` mediante el Pull Request automático generado por el pipeline del repositorio de código.
