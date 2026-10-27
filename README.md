# DiceBound Infrastructure

Инфраструктура для развёртывания веб-приложения DiceBound с использованием Docker Compose и GitHub Container Registry (GHCR).

## Структура проекта

- `compose/production.yml` — конфигурация production-окружения.
- `.env.example` — пример необходимых переменных окружения.
- `.gitignore` — исключение локальных конфигураций и секретов из Git.

## Сервисы

**Backend**
- NestJS + Prisma.
- Порт внутри контейнера: `3000`.
- Публикуется на `127.0.0.1:3002` хоста.
- Подключается к существующей PostgreSQL через `DATABASE_URL`.
- Имеет healthcheck `/health`.

**Frontend**
- React + Vite, production-сборка раздаётся через Nginx.
- Порт внутри контейнера: `80`.
- Публикуется на `127.0.0.1:8080` хоста.

PostgreSQL не входит в Docker Compose, поскольку используется существующая база данных сервера.

## Конфигурация

Для локальной подготовки конфигурации:

1. Скопировать `.env.example` в `.env`.
2. Заполнить переменные окружения актуальными значениями.
3. Убедиться, что Docker имеет доступ к необходимым образам в GHCR.

Не коммитить `.env`, пароли, токены или реальные реквизиты подключения.

## Проверка Docker Compose

```bash
docker compose --env-file .env -f compose/production.yml config --quiet
```

Команда проверяет корректность конфигурации, не запуская контейнеры.

## Запуск

После согласования серверных настроек:

```bash
docker compose --env-file .env -f compose/production.yml pull
docker compose --env-file .env -f compose/production.yml up -d
docker compose --env-file .env -f compose/production.yml ps
```

Команды предназначены для подготовленного окружения, а не для запуска с демонстрационными значениями из `.env.example`.

## Дальнейшие работы

- Согласовать сетевое подключение к серверной PostgreSQL.
- Настроить reverse proxy, HTTPS и маршрутизацию API.
- Организовать автоматический деплой через GitHub Actions и Tailscale/SSH.
- Определить порядок применения миграций Prisma и обновления контейнеров.
- Добавить проверки работоспособности и процедуру отката.

Production-развёртывание пока не завершено.