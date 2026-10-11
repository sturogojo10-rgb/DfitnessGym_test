# D'fitness Gym

Laravel and XAMPP MySQL gym management project. The first phase provides a shared login for admins and members, member registration and approval, and role-based dashboards.

Follow [PROJECT_GUIDE.md](PROJECT_GUIDE.md) for setup, testing, and the step-by-step plan based on the supplied flowchart.

## Run with Docker

Build the application image from the project root:

```sh
docker build -t dfitness-gym .
```

Configure an `.env` file with a generated `APP_KEY` and the database credentials. When connecting to XAMPP MySQL on the host from Docker Desktop, set `DB_HOST=host.docker.internal` (rather than `127.0.0.1`). Then start the app:

```sh
docker run --rm --name dfitness-gym -p 8080:8080 --env-file .env dfitness-gym
```

Open <http://localhost:8080>. The database is not included in the image; run migrations with `docker exec -it dfitness-gym php artisan migrate --force` after starting the container.

For a Render deployment, configure `APP_ENV=production`, `APP_DEBUG=false`, `APP_URL`, `APP_KEY`, and the `DB_*` values in the service's environment settings. Set `SESSION_DRIVER=file`, `CACHE_STORE=file`, and `QUEUE_CONNECTION=sync` unless you have created the corresponding database tables or configured external services. Render environment settings override the defaults in this repository. Back up and review pending migrations before running them against a database with existing data.
