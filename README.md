# Momo Store Project – Docker Deployment

Этот проект представляет собой веб-приложение "Momo Store" с backend на Go и frontend на Vue.js.

## Структура проекта

- **backend/** – Go REST API сервер.
- **frontend/** – Vue.js одностраничное приложение (SPA).
- **docker-compose.yml** – оркестрация контейнеров (backend, frontend, БД, proxy).
- **nginx.conf** – конфигурация Nginx для реверс-прокси (балансировка нагрузки).
- **.github/workflows/deploy.yaml** – CI/CD workflow для сборки, сканирования и деплоя Docker-образов.
- **README.md** – текущее описание и инструкции.

## Быстрый старт

Требования: Docker и Docker Compose установлены локально.

1. **Настройка переменных:** Создайте файл `.env` в корне или экспортируйте переменные окружения:
   ```bash
   # .env файл пример
   DB_PASSWORD=<пароль для БД>
   ```
