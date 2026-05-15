# Super Pomodoro

## Requirements

- Docker / Docker desktop
- [Frontend App](https://github.com/DiosDuck/super-pomodoro-frontend)
- [Backend App](https://github.com/DiosDuck/super-pomodoro-backend)

## Installation

Download Backend App and put it inside of a new folder called 'backend'.
Download Frontend App and put it inside of a new folder called 'frontend'.
Example inside the current folder:
```
Super Pomodoro
|
|_backend -> backend project
|   |_...
|_frontend -> frontend project
|   |_...
|_locale -> generated
|   |_...
|_docker
|   |_...
|_.gitignore
|_docker-compose.yml
|_README.md
```

Run the following commands:

```
docker compose up -d --build
```

After that, use the `php_tools` container to execute the following scripts (add sudo if required):
```
# composer install
docker compose --profile tools run --rm php_tools composer install
# update database
docker compose --profile tools run --rm php_tools php bin/console doctrine:migration:migrate
# adding the JWT tokens
docker compose --profile tools run --rm php_tools php bin/console lexik:jwt:generate-keypair
```

By default the project starts in `dev` mode. Wait for the `frontend` container to finish `npm ci` and the initial `ng serve` compile, then access the page at `localhost:4201`.

To stop everything: `docker compose down`.

## Walkthrough the app

### Modes

The app supports 2 modes (implemented as Docker Compose profiles): `dev` (default) and `demo`. To change the default mode, edit the `.env` file at the repo root:

```
COMPOSE_PROFILES=dev
```

You can also override it for a single command, e.g. `COMPOSE_PROFILES=demo docker compose up -d --build`.

### Common locations

- Symfony/BE application is reached through `localhost:8081/api/{url}`, with Swagger API documentation at `localhost:8081/api/doc`
- Mailpit web UI is reachable at `localhost:8025` (SMTP listens on `localhost:1025`)
- MySQL is reachable at `localhost:3307` with credentials `root:root` (root user) or `user:password` (non-root); the default database is `sp_dev`
- `php_tools` is a separate container for Symfony CLI work. It is used to install dependencies and execute console scripts
- XDEBUG is enabled by default in this project, to allow debugging on Apache and `php_tools`

### DEV mode

- Angular/FE application is reached at `localhost:4201` and runs through `ng serve` together with the proxy config (`frontend/proxy.conf.json`), which forwards `/api/*` calls to Apache. This avoids any CORS limitation and enables all standard debugging features (HMR, dev source maps)
- To use it, either keep the default `.env` (`COMPOSE_PROFILES=dev`) or execute:
```
docker compose --profile dev up -d --build
```

### DEMO mode

- Angular/FE application is reached at `localhost:8081` and is built through `ng build --watch`, producing a static copy served by Apache — imitating the flow used on PROD
- To use it, either change `.env` to `COMPOSE_PROFILES=demo` or execute:
```
docker compose --profile demo up -d --build
```
