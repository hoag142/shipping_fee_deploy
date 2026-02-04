# Shipping Fee Deployment

Docker deployment for the Shipping Fee application (GHN API integration).

## Prerequisites

- Docker & Docker Compose installed
- Git installed

## Quick Start

### 1. Clone this repository

```bash
git clone https://github.com/hoag142/shipping_fee_deploy.git
cd shipping_fee_deploy
```

### 2. Clone application repositories

```bash
git clone https://github.com/hoag142/shipping_fee.git
git clone https://github.com/hoag142/shipping_fee_client.git
```

### 3. Configure environment

```bash
cp .env.example .env
nano .env  # Add your GHN API credentials
```

### 4. Build and run

```bash
docker-compose up -d --build
```

### 5. Access the application

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
├── shipping_fee/           # Backend repo (cloned)
└── shipping_fee_client/    # Frontend repo (cloned)
```

## Update Application

```bash
# Pull latest changes
cd shipping_fee && git pull && cd ..
cd shipping_fee_client && git pull && cd ..

# Rebuild and restart
docker-compose up -d --build
```
