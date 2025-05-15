# Momo Store Project – Docker Deployment

Этот проект представляет собой веб-приложение "Momo Store" с backend на Go и frontend на Vue.js.

## Структура проекта

- **backend/** – Go REST API сервер.
- **frontend/** – Vue.js одностраничное приложение (SPA).
- **docker-compose.yml** – оркестрация контейнеров (backend, frontend, proxy).
- **nginx.conf** – конфигурация Nginx для реверс-прокси.
- **.github/workflows/deploy.yaml** – CI/CD workflow для сборки, сканирования и деплоя Docker-образов.
- **README.md** – текущее описание и инструкции.

## Быстрый старт

Требования: Docker и Docker Compose установлены локально.

Запустить приложение:
```bash
docker compose --profile production build
docker compose --profile production up -d
```