# Проверка по чек-листу

1. **PASS** — Dockerfile: `USER node`, пользователь не root — `Dockerfile:11`; `/data` передаётся `node:node` до смены пользователя — `Dockerfile:8`.
2. **PASS** — Compose: host-порт привязан к `127.0.0.1` — `compose.yaml:9`.
3. **PASS по проверенным файлам** — секретов в `ENV`/`ARG` Dockerfile и `environment` Compose нет; секрет передаётся через `secrets` — `compose.yaml:12–13, 24–26`. Файл секрета исключён из Docker-контекста — `.dockerignore:3`.
4. **НЕ УДАЛОСЬ ПРОВЕРИТЬ** — `report/scout.txt:1` сообщает, что Scout не установлен и проверка не выполнена; Scout не запускался.
5. **PASS** — логин и токен Workflow заданы только через `secrets.NAME` — `.github/workflows/build.yml:21–22`.

## Решения

- Пункт 4, Scout — принято: Scout на этом компьютере не установлен, проверка уязвимостей не сделана. Перед следующим выпуском поставить Scout и снять `report/scout.txt` заново; в этот выпуск правок нет.
- Пункты 1, 2, 3, 5 — согласен, сверил с файлами: `USER node` и `chown node:node /data` выше него в Dockerfile, `127.0.0.1:8080:3000` в compose.yaml, `api_token.txt` в .dockerignore и секрет через `secrets`, `secrets.DOCKERHUB_USERNAME` и `secrets.DOCKERHUB_TOKEN` в build.yml.
