# Plato Example App

A 3-container Docker Compose setup demonstrating:
- **PostgreSQL Database** (port 5432)
- **Static HTML Test App** (port 8080)  
- **Nginx Reverse Proxy** (port 80)

## Quick Start

```bash
# Start all services
docker-compose up -d

# View logs
docker-compose logs -f

# Stop all services
docker-compose down
```

## Services

### 1. Database (`plato_db`)
- **Image**: postgres:latest
- **Port**: 5432
- **Credentials**: user/password
- **Database**: mydb

### 2. Test App (`plato_test_app`)
- **Port**: 8080
- **Health**: http://localhost:8080/health
- **Content**: Static HTML with modern styling

### 3. Nginx (`plato_nginx`)
- **Port**: 80 (main gateway)
- **Routes**:
  - `/` - Nginx landing page
  - `/app/` - Proxy to test app
  - `/test` - Alternative proxy to test app
  - `/health` - Nginx health check

## URLs

- **Main Gateway**: http://localhost/
- **Test App Direct**: http://localhost:8080/
- **Test App via Proxy**: http://localhost/app/
- **Health Checks**: http://localhost/health

## Database Connection

```bash
# Connect to PostgreSQL
docker exec -it plato_db psql -U user -d mydb

# Or from host (if psql installed)
psql -h localhost -U user -d mydb
```

## Architecture

```
Client Request (port 80)
    ↓
Nginx Reverse Proxy
    ↓
Static Test App (port 8080)
    ↓
PostgreSQL Database (port 5432)
```

All containers use `network_mode: host` for simplified networking within the Plato simulation environment.
