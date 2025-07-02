# CarFactory.DevOps

## Local Development Setup with Docker

This guide will help you set up and run the application locally using Docker Compose.

## Prerequisites

- Docker
- Docker Compose

## Getting Started

### 1. Environment Configuration

Before running the application, you need to create and configure your environment file:

1. Copy the example environment file:
   ```bash
   cp .env-example .env
   ```

2. Edit the `.env` file and configure the following variables according to your setup:

#### Database Configuration
- **`DB_PORT`** - Port number for the database connection (e.g., `5432` for PostgreSQL, `3306` for MySQL, `1433` for MSSQL)
- **`DB_NAME_EMPLOYEES`** - Name of the database for employees service
- **`DB_NAME_FACTORIES`** - Name of the database for factories service
- **`DB_USERNAME`** - Database username for authentication
- **`DB_PASSWORD`** - Database password for authentication

#### API Configuration
- **`API_EMPLOYEES_ENVIRONMENT`** - Environment setting for the employees API (e.g., `development`, `production`)
- **`API_EMPLOYEES_PORT`** - Port number where the employees API will run
- **`API_FACTORIES_PORT`** - Port number where the factories API will run

#### Docker Hub Configuration
- **`DOCKER_HUB`** - Docker Hub registry URL (usually `docker.io` or leave empty for default)
- **`DOCKER_HUB_REPOSITORY`** - Your Docker Hub repository name
- **`DOCKER_HUB_EMPLOYEES_TAG`** - Docker image tag for the employees service
- **`DOCKER_HUB_FACTORIES_TAG`** - Docker image tag for the factories service

### 2. Running the Application

Once your `.env` file is properly configured, start the application using Docker Compose:

```bash
docker-compose up -d
```

This command will:
- Pull the necessary Docker images
- Create and start all required containers
- Set up the network connections between services

### 3. Accessing the Services

After successful startup, you can access:
- Employees API: `http://localhost:${API_EMPLOYEES_PORT}`
- Factories API: `http://localhost:${API_FACTORIES_PORT}`

### 4. Stopping the Application

To stop all services:

```bash
docker-compose down
```

## Troubleshooting

- Make sure all required environment variables are set in your `.env` file
- Verify that the specified ports are not already in use by other applications
- Check Docker logs if services fail to start: `docker-compose logs [service-name]`

## Notes

- The `.env-example` file in this repository contains sample values that you can use as a reference
- Make sure your `.env` file is not committed to version control as it may contain sensitive information
