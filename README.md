# 3x-ui за существующим Traefik

Панель использует сеть хоста. Контейнер panel-proxy подключён к внешней сети
proxy и передаёт HTTP-запросы на порты панели через host.docker.internal.
Traefik завершает TLS; сертификат внутри 3x-ui не требуется.

Условия для существующего Traefik:

- Docker provider смотрит на Docker Engine этого сервера.
- Сеть proxy существует; для overlay включён attachable.
- Есть entrypoint websecure и ACME resolver letsencrypt.
- Для HTTP challenge домен указывает на этот сервер и доступен TCP-порт 80.
- Метка ingress.edge=true позволяет пройти фильтр текущего Traefik.

В .env задайте DOMAIN без схемы и пути, PANEL_PORT и SUB_PORT согласно
фактическим HTTP-портам 3x-ui. SUB_PATH должен совпадать с путём подписок
в настройках панели. Изменение .env не меняет параметры в базе 3x-ui.
После восстановления базы проверьте порты и путь подписок снова.

Запуск из этой папки:

```bash
docker compose config --quiet
docker compose up -d
```

В новой панели базовый путь сейчас /, поэтому адрес входа — https://DOMAIN/.
Прежний /grove-admin-7k92/ появится только после его настройки в самой панели.
Порты nginx 8080/8081 на хост не публикуются.
