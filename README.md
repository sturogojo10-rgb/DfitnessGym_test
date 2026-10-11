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

For a Render deployment, use a Render PostgreSQL database. The free web-service filesystem is ephemeral, so a SQLite file stored in the container is not persistent across replacements.

1. Create a Render PostgreSQL database and connect it to the web service.
2. Set these service environment variables:

   ```text
   APP_ENV=production
   APP_DEBUG=false
   DB_CONNECTION=pgsql
   DB_URL=<the PostgreSQL URL provided by Render>
   LOG_CHANNEL=stderr
   SESSION_DRIVER=file
   CACHE_STORE=file
   QUEUE_CONNECTION=sync
   ```

   Also set the production `APP_URL` and `APP_KEY`. Render environment settings override image defaults. Keep `DB_URL` secret and use the database's internal URL for the deployed service.
3. Deploy. `LOG_CHANNEL=stderr` makes Laravel exceptions visible in the Render service logs. The Free plan does not provide a service shell, so run database migrations from a trusted local checkout using the database's external URL in its local `.env`, then restore the normal local database settings. Do not commit database URLs or credentials.

Back up and review pending migrations before running them against a database with existing data. In particular, `2026_10_07_000001_remove_rejected_member_accounts` deletes rejected member rows when it runs. The Docker image does not run migrations automatically.
