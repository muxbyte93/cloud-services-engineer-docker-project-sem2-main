# Stage 1: Build the Vue app
FROM node:18-alpine AS builder
WORKDIR /app

# Устанавливаем переменную как /api
ARG VUE_APP_API_URL            
ENV VUE_APP_API_URL=$VUE_APP_API_URL

# Устанавливаем переменную для обхода ошибки OpenSSL
ENV NODE_OPTIONS=--openssl-legacy-provider

# Установим зависимости
COPY package*.json ./
RUN npm ci

COPY . .
RUN npm run build

# Stage 2: Nginx stage to serve content
FROM nginx:1.25-alpine

# Удаляем дефолтную конфигурацию
RUN rm -rf /etc/nginx/conf.d/default.conf
# Копируем кастомный конфиг для /momo-store/
COPY frontend-nginx.conf /etc/nginx/conf.d/default.conf

# Копируем собранные файлы фронтенда в подпапку /momo-store
RUN mkdir -p /usr/share/nginx/html/momo-store
COPY --from=builder /app/dist /usr/share/nginx/html/momo-store

EXPOSE 80

# Healthcheck проверяет доступность именно /momo-store/
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s \
  CMD wget -q -O /dev/null http://127.0.0.1/momo-store/ || exit 1
