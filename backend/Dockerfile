# Stage 1: Build the Go application
FROM golang:1.20-alpine AS builder
WORKDIR /app

# Установим модули зависимостей
COPY go.mod go.sum ./
RUN go mod download

# Копируем исходный код
COPY . .

# Компилируем бинарный файл
RUN CGO_ENABLED=0 go build -o /app/bin/app ./cmd/api

# Stage 2: Final minimal image
FROM alpine:3.17

# Устанавливаем netcat для healthcheck
RUN apk add --no-cache netcat-openbsd

# Создаем системного пользователя без привилегий
RUN addgroup -S appgroup && adduser -S appuser -G appgroup

WORKDIR /app
COPY --from=builder /app/bin/app ./app

# Используем непривилегированного пользователя
USER appuser

# Открываем порт приложения
EXPOSE 8081

# Healthcheck
HEALTHCHECK --interval=30s --timeout=5s --start-period=5s \ 
  CMD sh -c "nc -z localhost 8081 || exit 1"

# Команда запуска контейнера
CMD ["./app"]