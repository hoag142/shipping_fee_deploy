# Shipping Fee Deployment

Docker deployment for the Shipping Fee application (GHN API integration).

## Prerequisites

- Docker & Docker Compose installed
- Git installed

## Quick Start

### 1. Clone this repository (with submodules)

```bash
git clone --recurse-submodules https://github.com/hoag142/shipping_fee_deploy.git
cd shipping_fee_deploy
```

Or if already cloned without submodules:
```bash
git submodule update --init --recursive
```

### 2. Configure environment

```bash
cp .env.example .env
nano .env  # Add your GHN API credentials
```

### 3. Build and run

```bash
docker-compose up -d --build
```

### 4. Access the application

- **Frontend**: http://localhost (or http://your-server-ip)
- **Backend API**: http://localhost:8080/api/

## Useful Commands

```bash
# View logs
docker-compose logs -f

# View specific service logs
docker-compose logs -f backend
docker-compose logs -f frontend

# Stop services
docker-compose down

# Rebuild and restart
docker-compose up -d --build

# Restart specific service
docker-compose restart backend
```

## Project Structure

```
shipping_fee_deploy/
├── docker-compose.yml
├── .env.example
├── .env                    # Your local config (not committed)
├── .gitmodules             # Submodule configuration
├── shipping_fee/           # Backend (git submodule)
└── shipping_fee_client/    # Frontend (git submodule)
```

## Update Application

```bash
# Pull latest submodule changes
git submodule update --remote --merge

# Rebuild and restart
docker-compose up -d --build
```
