# Spring Boot Shiro demo

This project runs as a Docker Compose stack with the Spring Boot app, MySQL, and Redis. Docker Compose waits for both services to pass their health checks before starting the app.

## Start

Install Docker Desktop (or Docker Engine with the Compose plugin), then run from the project directory:

```sh
docker compose up --build -d
docker compose logs -f app
```

Open [http://localhost:8080](http://localhost:8080). Set `APP_PORT` to change the host port. Compose uses development-only credentials by default; override `MYSQL_PASSWORD`, `MYSQL_ROOT_PASSWORD`, and optionally `REDIS_PASSWORD` before deployment. Keep those values private and use strong secrets.

For example, in PowerShell:

```powershell
$env:MYSQL_PASSWORD = "replace-with-a-strong-password"
$env:MYSQL_ROOT_PASSWORD = "replace-with-a-different-strong-password"
$env:REDIS_PASSWORD = "replace-with-another-strong-password"
docker compose up --build -d
```

The MySQL schema and sample records are loaded the first time the `mysql-data` volume is created. To keep data across restarts, leave the volume in place. To deliberately start with a fresh database, run `docker compose down -v` (this deletes the MySQL and Redis data).

## Operations

```sh
docker compose ps
docker compose logs -f app
docker compose down
```

The app is built inside Docker with Java 8 and runs as an unprivileged user. Database and Redis endpoints are configured for the Compose network and can also be overridden with Spring's standard environment variables. For an external database, set `SPRING_DATASOURCE_URL`, `SPRING_DATASOURCE_USERNAME`, and `SPRING_DATASOURCE_PASSWORD`; initialize it with `src/main/resources/db/schema-mysql.sql` and, if wanted, `src/main/resources/db/data-mysql.sql`.
