# REBEL Fitness

A full-stack web application for tracking workouts, exercises, and fitness goals.
It consists of a Laravel REST API and a React single-page application (SPA) with
role-based access control (guest, member, admin).

> Originally built as a term project for the *Internet Technologies* course at the
> Faculty of Organisational Sciences, University of Belgrade. It is now maintained
> as a personal learning project for practicing modern development and DevOps workflows.

## Features

The application has three user roles:

**Guest**
- Start a guest session without registering
- Browse public workouts and view their details (read-only)

**Member**
- Register and log in
- Create, edit, and delete personal workouts
- Add exercises to workouts
- Filter and search exercises by type and keywords
- Browse public workouts (read-only)

**Admin**
- Everything a member can do
- View and delete users
- Manage fitness goals
- View and manage all workouts

## Tech Stack

| Layer    | Technology                                       |
|----------|--------------------------------------------------|
| Backend  | Laravel 11 (PHP 8.2+), Laravel Sanctum, Eloquent |
| Database | MySQL / MariaDB                                  |
| Frontend | React 19, Vite, React Router, Context API        |
| Styling  | Custom CSS (dark theme)                          |

## Project Structure

```
fitnessWebApp/
├── backend/     # Laravel 11 API (REST, Sanctum authentication)
├── frontend/    # React 19 + Vite SPA
└── docs/        # documentation and archived reports
```

Each directory is a self-contained application: `backend/` is the Laravel project
root, `frontend/` is the Vite project root.

## Getting Started

### Prerequisites

- PHP >= 8.2 and [Composer](https://getcomposer.org/)
- MySQL 8+ (or MariaDB)
- Node.js >= 20.19 and npm

### Backend

```bash
cd backend

# 1. Install PHP dependencies
composer install

# 2. Create the environment file and generate the application key
touch .env
php artisan key:generate

# 3. Set the database credentials in .env
#    DB_CONNECTION=mysql
#    DB_HOST=127.0.0.1
#    DB_PORT=3306
#    DB_DATABASE=fitness_web_app
#    DB_USERNAME=<your-user>
#    DB_PASSWORD=<your-password>

# 4. Create the database, then run migrations and seeders
mysql -u <your-user> -p -e "CREATE DATABASE fitness_web_app"
php artisan migrate --seed

# 5. Start the development server
php artisan serve
```

The API is now available at `http://127.0.0.1:8000`.

The seeder creates demo data: one admin account, several member accounts, and a
set of workouts, exercises, and goals.

### Seeded accounts (local development only)

| Role  | Email                | Password |
|-------|----------------------|----------|
| Admin | admin@fitnessapp.com | password |

### Frontend

```bash
cd frontend

# 1. Install dependencies
npm install

# 2. Start the development server
npm run dev
```

The application is now available at `http://localhost:5173`.

The frontend reads the API base URL from the `VITE_API_BASE` variable in
`frontend/.env` (default: `http://127.0.0.1:8000/api`).

## API Overview

Authentication uses bearer tokens issued by Laravel Sanctum.

| Method                     | Endpoint                   | Description                      | Access        |
|----------------------------|----------------------------|----------------------------------|---------------|
| POST                       | `/api/register`            | Create an account                | Public        |
| POST                       | `/api/login`               | Log in and receive an API token  | Public        |
| POST                       | `/api/guest/login`         | Create a temporary guest session | Public        |
| GET                        | `/api/weather/{city}`      | Current weather (OpenWeatherMap) | Public        |
| GET                        | `/api/workouts`            | List workouts                    | Any logged-in role |
| GET                        | `/api/workouts/{id}`       | Workout details                  | Any logged-in role |
| GET / POST                 | `/api/users/workouts`      | List / create own workouts       | Member or admin |
| PUT / DELETE               | `/api/users/workouts/{id}` | Update / delete own workout      | Member or admin |
| GET / POST / PUT / DELETE  | `/api/exercises`           | Manage exercises                 | Member or admin |
| GET / DELETE               | `/api/admin/users`         | List / delete users              | Admin         |
| GET / POST / PUT / DELETE  | `/api/goals`               | Manage goals                     | Admin         |

## Roadmap

The project is being modernized step by step. Planned work:

- [ ] Environment configuration (`.env.example` files, configuration cleanup)
- [ ] Docker images for the backend and the frontend
- [ ] Local development environment with Docker Compose
- [ ] CI/CD pipeline with GitHub Actions
- [ ] Kubernetes deployment (homelab cluster)
- [ ] Deployment to AWS (free tier)
