Запуск:

cp .env.example .env
docker-compose up -d --build
docker-compose ps
curl -i http://127.0.0.1:8080/readyz
curl -i http://127.0.0.1:8080/

Настройки APP_PORT и GREETING в .env

Сохранение данных:

curl -sS -X POST http://127.0.0.1:8080/increment
curl -sS -X POST http://127.0.0.1:8080/increment
curl -sS http://127.0.0.1:8080/
docker-compose down
docker-compose up -d
curl -sS http://127.0.0.1:8080/

Остановка и восстановление redis:

docker-compose stop redis
curl -i http://127.0.0.1:8080/healthz
curl -i http://127.0.0.1:8080/readyz
curl -i -X POST http://127.0.0.1:8080/increment
docker-compose start redis
curl -i http://127.0.0.1:8080/readyz
curl -sS http://127.0.0.1:8080/
curl -sS -X POST http://127.0.0.1:8080/increment

Лог docker-compose logs app redis:

app-1 | 2026-11-24 16:55:32,522 listening port=8000 storage=redis
app-1 | 2026-11-24 16:55:46,033 http 172.18.0.1 "GET /healthz HTTP/1.1" 200 -
app-1 | 2026-11-24 16:55:46,169 http 172.18.0.1 "GET /readyz HTTP/1.1" 503 -
app-1 | 2026-11-24 16:55:46,188 http 172.18.0.1 "POST /increment HTTP/1.1" 503 -
app-1 | 2026-11-24 16:55:57,104 http 172.18.0.1 "POST /increment HTTP/1.1" 200 -
redis-1 | 1:M 24 Nov 2026 16:55:57.074 * Ready to accept connections tcp

Результаты тестов:

1. Сборка работает, и app, и redis healthy
2. Два POST увеличили счётчик до 2, после down/up GET вернул 2
3. Redis остановлен: /healthz выдал 200, /readyz и POST /increment выдали 503
4. Redis запущен: /readyz выдал 200, GET вернул 2, POST вернул 3 (приложение не перезапускалось вручную)