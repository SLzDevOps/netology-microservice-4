# Домашнее задание "Микросервисы: масштабирование" - `Фомичев Анатолий`

## Ссылка на Д3 - https://github.com/netology-code/micros-homeworks/blob/main/11-microservices-04-scaling.md

### Задача 1
```
Кластеризация
Предлагаемое решение: Kubernetes (совместно с Helm и External Secrets Operator)
Kubernetes (k8s) — это промышленный стандарт для оркестрации контейнеров. Он полностью закрывает все требования.
  
Архитектура взаимодействия
    A[Разработчик] -->|kubectl apply| B[Kubernetes API]
    C[CI/CD Pipeline] -->|helm upgrade| B
    B --> D[Ingress Controller]
    D --> E[Service]
    E --> F[Pod 1]
    E --> G[Pod 2]
    H[Horizontal Pod Autoscaler] -->|scale| F
    I[External Secrets Operator] -->|fetch secrets| J[Vault / AWS Secrets Manager]
    J -->|inject| K[Pod environment]
    L[ConfigMap / Secret] -->|env vars| K
```
![alt text](https://github.com/SLzDevOps/netology-microservice-4/blob/main/Схема_ДЗ_микросервисы4.png).


| Требование | Реализация в Kubernetes |
| :--- | :--- |
Поддержка контейнеров | Нативная (Docker, containerd, CRI-O)
Обнаружение сервисов и маршрутизация запросов | Services (ClusterIP, LoadBalancer) + Ingress / Gateway API + CoreDNS
Горизонтальное масштабирование | Увеличение числа реплик в Deployment / StatefulSet
Автоматическое масштабирование | Horizontal Pod Autoscaler (по CPU, RAM, custom метрикам), Vertical Pod Autoscaler
Явное разделение внешних и внутренних ресурсов | Services с типами: ClusterIP (внутренний), LoadBalancer / NodePort / Ingress (внешний), Network Policies для изоляции трафика
Конфигурация через переменные среды | ConfigMap, Secret (base64), envFrom
Безопасное хранение чувствительных данных | Secrets (зашифрованы в etcd) + External Secrets Operator (интеграция с HashiCorp Vault, AWS Secrets Manager, GCP Secret Manager)

```
Дополнительно в стеке:
- Helm — шаблонизация и управление релизами (удобно описывать микросервисы)
- Ingress NGINC / Contour / Istio Gateway — для маршрутизации внешнего трафика
- cert-manager — автоматические TLS-сертификаты
- KEDA — событийно-управляемое масштабирование (по длине очереди Kafka/RabbitMQ)
- Secrets Store CSI Driver — монтирование секретов из внешних хранилищ в pod

Обоснование выбора:
- Доминирующий стандарт — подходит для любых микросервисных систем, поддерживается всеми облачными провайдерами (EKS, GKE, Yandex Managed K8s).
- Гибкость — можно запускать на своих серверах (kubeadm, Rancher, OpenShift) или в облаке.
- Разделение ресурсов — достигается через Namespaces, Network Policies, ResourceQuotas, LimitRanges.
- Автомасштабирование — от простого HPA до продвинутого KEDA.
- Безопасность — встроенные Secrets + интеграция с внешними менеджерами через ESO.
- Экосистема — огромное количество готовых операторов (PostgreSQL, Redis, Prometheus и др.).
```

### Задача 2

Ссылка на docker-compose.yml - https://github.com/SLzDevOps/netology-microservice-4/blob/main/docker-compose.yaml

![alt text](https://github.com/SLzDevOps/netology-microservice-4/blob/main/Screenshot_916.png).
![alt text](https://github.com/SLzDevOps/netology-microservice-4/blob/main/Screenshot_917.png).



