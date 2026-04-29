# Docker Setup for Wanderlust

This guide explains how to run the Wanderlust project using Docker Compose.

## Prerequisites

- [Docker](https://www.docker.com/get-started) installed
- [Docker Compose](https://docs.docker.com/compose/install/) installed

## Quick Start

1. **Copy the environment file:**

   ```bash
   cp .env.example .env
   ```

2. **Start all services:**

   ```bash
   docker-compose up -d
   ```

3. **Access the application:**
   - Frontend: http://localhost:5173
   - Backend API: http://localhost:5000

## Services

| Service   | Port | Description          |
|-----------|------|----------------------|
| frontend  | 5173 | React frontend       |
| backend   | 5000 | Express API server   |
| mongodb   | 27017| MongoDB database     |
| redis     | 6379 | Redis cache          |

## Common Commands

```bash
# Start services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop services
docker-compose down

# Rebuild containers
docker-compose build --no-cache

# Stop and remove volumes (reset database)
docker-compose down -v
```

## Environment Variables

Edit `.env` to configure:

- `JWT_SECRET` - Secret key for JWT tokens (change in production!)
- `GOOGLE_CLIENT_ID` / `GOOGLE_CLIENT_SECRET` - For Google OAuth (optional)
- Other variables as needed

## Development

The Docker setup mounts your local files, so changes to source code will reflect in the containers (hot reload enabled).