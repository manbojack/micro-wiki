# MicroWiki — микросервисная вики-платформа

## 🧰 Стек

<p>
  <img src="https://img.shields.io/badge/Python-FastAPI-3776AB?logo=python&logoColor=white" />
  <img src="https://img.shields.io/badge/Go-1.20-00ADD8?logo=go&logoColor=white" />
  <img src="https://img.shields.io/badge/Kubernetes-K8s-326CE5?logo=kubernetes&logoColor=white" />
  <img src="https://img.shields.io/badge/Redis-Cache-DC382D?logo=redis&logoColor=white" />
  <img src="https://img.shields.io/badge/Kafka-EventBus-231F20?logo=apachekafka&logoColor=white" />
  <img src="https://img.shields.io/badge/PostgreSQL-DB-336791?logo=postgresql&logoColor=white" />
  <img src="https://img.shields.io/badge/Docker-Container-2496ED?logo=docker&logoColor=white" />
  <img src="https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF?logo=githubactions&logoColor=white" />
  <img src="https://img.shields.io/badge/Grafana-Monitoring-F46800?logo=grafana&logoColor=white" />
  <img src="https://img.shields.io/badge/VirtualBox-VMs-183A61?logo=virtualbox&logoColor=white" />
</p>

---

## 📌 Описание

**MicroWiki** — это мини-вики-платформа с микросервисной архитектурой, включающая:

- статьи, редактирование и история версий  
- комментарии и авторизация  
- события через Kafka и кэш через Redis  
- сбор метрик, логов и алертов  
- автосборка и деплой через GitHub Actions

Разворачивается локально в кластере **Kubernetes**, собранном на **VirtualBox**-машинах с помощью **Ansible**.

---

## ⚙️ Архитектура

<details>
<summary>📐 Диаграмма</summary>

```text
User
 │
 ▼
Frontend (React/HTML)
 │
 ▼
API Gateway (NGINX)
 ├────────▶ Article Service (FastAPI)
 ├────────▶ Comment Service (FastAPI)
 └────────▶ Auth Service (Go)

        └─▶ PostgreSQL
        └─▶ Redis (кэш)
        └─▶ Kafka (события)
        └─▶ VictoriaMetrics + Grafana (мониторинг)
```
</details>

## 📁 Структура проекта
```
micro-wiki/
├── services/
│   ├── article-service/      # FastAPI
│   ├── comment-service/      # FastAPI
│   └── auth-service/         # Go
├── k8s/                      # YAML-манифесты Kubernetes
├── charts/                   # Helm чарты (опционально)
├── ansible/                  # Автоматизация установки
├── .github/workflows/ci.yml  # CI/CD pipeline (GitHub Actions)
└── README.md
```

## 🚀 Развертывание
1. Локальные VM (VirtualBox)    
    - 3 виртуалки Ubuntu 20.04  
    - По 2 vCPU и 2–3 GB RAM  
    - NAT + Host-only сети  

2. Kubernetes (через Ansible)  
    ```bash
    ansible-playbook ansible/setup-k8s.yaml -i ansible/inventory.ini
    ```

3. Helm-зависимости
    ```bash
    helm repo add bitnami https://charts.bitnami.com/bitnami
    helm install kafka bitnami/kafka
    helm install redis bitnami/redis
    helm install postgresql bitnami/postgresql
    ```

4. CI/CD через GitHub Actions
    - Workflow: `.github/workflows/ci.yml`
    - Автоматически:
      - билд Docker-образов
      - пуш в Docker Hub (или локальный registry)
      - деплой в кластер через kubectl / ansible

## 📊 Мониторинг
  - 📈 Grafana: `http://<vm-ip>:3000`
  - Метрики из: `Node Exporter`, `VictoriaMetrics` приложений

## 🧪 Разработка
Пример запуска FastAPI-сервиса локально:
  ```
cd services/article-service
uvicorn main:app --reload --port 8001
  ```

## 📌 Roadmap
  - [ ] История редактирования статей через Kafka events
  - [ ] Авторизация через OAuth2 / OpenID
  - [ ] Live-комментарии через WebSocket
