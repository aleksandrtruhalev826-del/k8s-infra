# k8s-infra

Kubernetes-кластер и экспорт манифестов. Публичный проект: **[devops1988.website](https://devops1988.website/)** (в кластере: `shop.devops1988.website`).

## Сайт в Kubernetes

| | |
|---|---|
| **URL** | https://devops1988.website/ |
| **Ingress** | `shop-ingress` → host `shop.devops1988.website` |
| **Reverse proxy** | NGINX (`Deployment/nginx`, NodePort `30081`) + маршрутизация на `frontend` |
| **Frontend** | Online Boutique (`frontend`, NodePort `32008`) |
| **Язык сервисов** | **Go** (микросервисы demo: checkout, cart, payment и др.) |

## Кластер

| Параметр | Значение |
|----------|----------|
| Kubernetes | **v1.32.13** |
| Нод | **4** — `k8s-cp` (control-plane), `k8s-node2`, `k8s-node3`, `node1` |
| Подов | **66** всего, **50** в статусе Running |
| Workloads (Deploy/STS/DS) | **36** |
| CNI | Flannel |
| Мониторинг | kube-prometheus-stack (Prometheus, Grafana) |
| GitOps / secrets | Argo CD, HashiCorp Vault |

## Базы данных и брокеры

| Сервис | Namespace | Роль |
|--------|-----------|------|
| PostgreSQL 17 | `mediawiki` | MediaWiki |
| Redis | `default` | Корзина (`redis-cart`) |
| Redis | `argocd` | Argo CD |
| Kafka + Kafka UI | `default` | Стриминг / админ-UI |

Секреты в Git **не** хранятся; каталог `cluster-export/` — снимок без Secret.

## Сеть

    Internet → devops1988.website (DNS)
             → reverse proxy (NGINX / NodePort 30081)
             → Ingress shop-ingress / frontend:80
             → микросервисы (ClusterIP)

## Структура репозитория

- `cluster-export/` — YAML по namespace + Helm values (Vault, kube-prometheus-stack)
- `docs/devops1988-website.md` — описание для портфолио

## Клонирование

    git clone https://github.com/aleksandrtruhalev826-del/k8s-infra.git

## Автор

[Aleksandr](https://github.com/aleksandrtruhalev826-del)
