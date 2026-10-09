# Auth Project

A simple authentication project. Laravel (Sanctum) backend and an HTML/JS frontend.

## Project structure

```
auth-project/
├── backend/     Laravel API
├── frontend/    Frontend (HTML/JS)
├── API.md       API contract (endpoints, requests, responses)
└── README.md
```

## Run the backend

```bash
git clone https://github.com/mahdi10385/auth-project.git
cd auth-project/backend

composer install
cp .env.example .env
php artisan key:generate
php artisan migrate --seed
php artisan serve
```

## Test account

Created by the seeder:

| Field    | Value            |
|----------|------------------|
| Email    | test@example.com |
| Password | password123      |

## Important rules for API requests

Every request must include this header, otherwise Laravel redirects on errors instead of returning JSON:

```
Accept: application/json
```

Protected routes also need:

```
Authorization: Bearer YOUR_TOKEN
```

See `API.md` for all endpoints.

## Git workflow

1. Never push directly to `main`.
2. Create a branch for each task:
```bash
   git checkout main
   git pull
   git checkout -b feature/short-task-name
```
3. Commit with clear messages (`feat: ...`, `fix: ...`, `docs: ...`).
4. Push the branch and open a Pull Request on GitHub.
5. The other person reviews and merges it.
6. After merging, everyone runs `git checkout main && git pull`.

## After pulling new changes

```bash
cd backend
composer install
php artisan migrate
```

## Common problems

| Problem | Solution |
|---------|----------|
| `php` is not recognized | Add the XAMPP php folder to PATH |
| `could not find driver` | Enable `extension=pdo_mysql` in `php.ini` (XAMPP: `C:\xampp\php\php.ini`) and restart the terminal |
| `Connection refused` / `SQLSTATE[HY000] [2002]` | MySQL is not running. Start it from the XAMPP Control Panel |
| `Unknown database 'auth_project'` | Create the database first (see step 2) |
| `Access denied for user 'root'` | Check `DB_USERNAME` and `DB_PASSWORD` in `.env` |
| `Port 3306 already in use` | Another MySQL is running. Stop it or change `DB_PORT` in `.env` |
| CORS error in browser | Make sure the backend is running and the URL is `http://localhost:8000` |
| Validation errors redirect to an HTML page | Add the `Accept: application/json` header |
| `Unauthenticated` | Check the `Authorization: Bearer TOKEN` header |