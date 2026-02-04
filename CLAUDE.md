# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

Docker deployment orchestrator for a Shipping Fee Calculator integrating with GHN (Giao Hàng Nhanh) Vietnamese shipping carrier API. This repo coordinates two application repositories:
- `shipping_fee/` — Spring Boot backend (Java 17, Maven)
- `shipping_fee_client/` — React frontend (TypeScript, Vite)

## Build & Run Commands

### Docker (Full Stack)
```bash
docker-compose up -d --build          # Build and start all services
docker-compose logs -f                # View all logs
docker-compose logs -f backend        # Backend logs only
docker-compose logs -f frontend       # Frontend logs only
docker-compose down                   # Stop services
docker-compose restart backend        # Restart specific service
```

### Backend (Local Development)
```bash
cd shipping_fee
mvnw.cmd clean package                # Build JAR
mvnw.cmd spring-boot:run              # Run dev server (port 8080)
mvnw.cmd test                         # Run all tests
mvnw.cmd test -Dtest=ShippingFeeApplicationTests  # Run single test
```

### Frontend (Local Development)
```bash
cd shipping_fee_client
npm install                           # Install dependencies
npm run dev                           # Dev server (port 3000, proxied to 8080)
npm run build                         # Production build
npm run lint                          # ESLint check
```

## Architecture

```
┌─────────────────┐     ┌──────────────────┐     ┌─────────────────┐
│  Frontend       │     │  Backend         │     │  GHN API        │
│  (Nginx :80)    │────▶│  (Spring :8080)  │────▶│  (external)     │
│  React/Vite     │     │  REST API        │     │                 │
└─────────────────┘     └──────────────────┘     └─────────────────┘
```

**Backend Layers:**
- `controller/ShippingController.java` — REST endpoints at `/api/shipping/*`
- `service/GhtkService.java` — GHN API integration with fallback to mock data
- `dto/` — Request/response objects with `ApiResponse<T>` wrapper
- `config/WebConfig.java` — CORS for dev servers (ports 3000, 5173)

**Frontend Layers:**
- `components/ShippingForm.tsx` — Main form component
- `services/api.ts` — Centralized API calls
- `types/index.ts` — TypeScript interfaces matching backend DTOs

## API Endpoints

Base URL: `http://localhost:8080/api/shipping`

| Method | Endpoint | Description |
|--------|----------|-------------|
| GET | `/provinces` | 63 Vietnamese provinces/cities |
| GET | `/districts/{provinceId}` | Districts by province |
| GET | `/wards/{districtId}` | Wards by district |
| POST | `/calculate` | Calculate shipping fee |
| GET | `/health` | Health check |

## Environment Configuration

Copy `.env.example` to `.env` and configure:
```
GHTK_API_TOKEN=your_ghn_token_here
GHTK_API_SHOP_ID=your_shop_id_here
GHTK_API_BASE_URL=https://dev-online-gateway.ghn.vn
```

Production gateway: `https://online-gateway.ghn.vn`

## Key Conventions

**Backend:**
- Lombok annotations: `@Slf4j`, `@RequiredArgsConstructor`, `@Data`, `@Builder`
- All responses use `ApiResponse<T>` wrapper
- Falls back to hardcoded Vietnamese mock data when GHN API unavailable
- Javadoc (`/** */`) for classes/methods, `//` for inline comments, no decorative comment characters

**Frontend:**
- TypeScript with strict mode
- Vietnamese UI labels (Tỉnh/Thành phố, Quận/Huyện, Phường/Xã)
- Loading and error states for all API calls
- Vite proxy forwards `/api` to backend

## Updating Applications

```bash
cd shipping_fee && git pull && cd ..
cd shipping_fee_client && git pull && cd ..
docker-compose up -d --build
```
