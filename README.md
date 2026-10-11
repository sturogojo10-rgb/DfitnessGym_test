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

For a Render deployment using SQLite:

1. Add a Render persistent disk to the web service, mounted at `/var/www/html/storage/app`.
2. Set these service environment variables:

   ```text
   APP_ENV=production
   APP_DEBUG=false
   DB_CONNECTION=sqlite
   DB_DATABASE=/var/www/html/storage/app/database.sqlite
   SESSION_DRIVER=file
   CACHE_STORE=file
   QUEUE_CONNECTION=sync
   ```

   Also set the production `APP_URL` and a generated `APP_KEY`. Render environment settings override image defaults.
3. Deploy, then use the Render Shell to run `php artisan migrate --force` once the disk is mounted. This creates the Laravel tables in the persistent SQLite database.

The image creates the SQLite file at startup if it does not already exist. The persistent disk keeps the database across deploys; without it, the database file is lost when Render replaces the container. Back up and review pending migrations before running them against a database with existing data. In particular, `2026_10_07_000001_remove_rejected_member_accounts` deletes rejected member rows when it runs.
