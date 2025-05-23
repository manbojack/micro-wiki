# 🧠 MicroWiki — мини-вики платформа на микросервисной архитектуре

Учебный проект для практики DevOps-инструментов, микросервисной архитектуры и CI/CD. Реализует простую платформу вики-статей с комментариями, авторизацией и мониторингом.

## 📦 Архитектура

Микросервисная архитектура, развернутая в Kubernetes-кластере на локальных виртуалках Ubuntu (VirtualBox). CI/CD обеспечивается через GitHub Actions + Ansible.

**Компоненты:**

- **Frontend**: простой HTML/React-интерфейс
- **API Gateway**: NGINX
- **Article Service** (Python/FastAPI)
- **Comment Service** (Python/FastAPI)
- **Auth Service** (Go)
- **Event Bus**: Kafka
- **Cache**: Redis
- **Database**: PostgreSQL
- **Monitoring**: VictoriaMetrics + Grafana + Node Exporter
- **CI/CD**: GitHub Actions + Docker + Ansible

## 🛠️ Стек технологий

| Категория     | Технология                      |
|---------------|----------------------------------|
| Backend       | Python (FastAPI), Go             |
| Orchestration | Kubernetes (kubeadm, VirtualBox) |
| CI/CD         | GitHub Actions, Docker, Ansible  |
| Database      | PostgreSQL                       |
| Caching       | Redis                            |
| Messaging     | Kafka                            |
| Monitoring    | VictoriaMetrics, Grafana         |
| Virtualization| VirtualBox (Ubuntu 20.04)        |

## 📂 Структура проекта

```
micro-wiki/
├── services/
│ ├── article-service/ # FastAPI
│ ├── comment-service/ # FastAPI
│ └── auth-service/ # Go
├── charts/ # Helm чарты
├── ansible/ # Playbooks
├── k8s/ # YAML-манифесты Kubernetes
├── .github/
│ └── workflows/
│ └── ci.yml # CI/CD pipeline (GitHub Actions)
├── README.md
└── docs/
```


## ⚙️ Развертывание (локально)

### 1. Виртуалки (VirtualBox)

- 3 VM с Ubuntu 20.04
- Ресурсы: 2 CPU, 2–3 GB RAM каждая
- Сети: NAT + Host-only

### 2. Kubernetes (через Ansible)

```bash
ansible-playbook ansible/setup-k8s.yaml -i ansible/inventory.ini
```

### 3. Установка зависимостей (Helm)
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm install kafka bitnami/kafka
helm install redis bitnami/redis
helm install postgresql bitnami/postgresql
```

### 4. CI/CD (GitHub Actions)
> При push в main запускается GitHub Actions pipeline
> Автоматически:
	- билдятся Docker-образы
	- пушатся в Docker Hub
	- деплоятся в кластер (через Ansible и kubectl)
