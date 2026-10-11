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
docker run --rm --name dfitness-gym -p 8080:80 --env-file .env dfitness-gym
```

Open <http://localhost:8080>. The database is not included in the image; run migrations with `docker exec -it dfitness-gym php artisan migrate --force` after starting the container.
