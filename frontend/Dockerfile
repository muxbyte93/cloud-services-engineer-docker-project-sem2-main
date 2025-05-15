# Stage 1: Build the Vue app
FROM node:18-alpine AS builder

RUN apk update && apk upgrade && apk add --no-cache python3 make g++

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

# переносим PID-файл из /var/run в /tmp
RUN mkdir -p /tmp && \
    sed -i 's|pid[[:space:]].*;|pid /tmp/nginx.pid;|' /etc/nginx/nginx.conf

# убираем дефолтный vhost и кладём свой
RUN rm -f /etc/nginx/conf.d/default.conf
COPY frontend-nginx.conf /etc/nginx/conf.d/default.conf

# создаём непривилегированного пользователя
RUN addgroup -S webapp && adduser -S -G webapp -s /sbin/nologin webapp

# статика SPA
RUN mkdir -p /usr/share/nginx/html/momo-store
COPY --from=builder --chown=webapp:webapp /app/dist /usr/share/nginx/html/momo-store

# права на каталоги, где nginx пишет файлы
RUN chown -R webapp:webapp /var/cache/nginx /var/log/nginx /etc/nginx/conf.d

USER webapp

EXPOSE 80

HEALTHCHECK --interval=30s --timeout=5s --start-period=5s \
  CMD wget -q -O /dev/null http://127.0.0.1/momo-store/ || exit 1
