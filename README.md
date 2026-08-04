# Growntrol Web

Vue 3 operational dashboard for Growntrol.

This repository contains only the browser application and its container image. Firmware, MQTT protocol, backend, simulator, broker configuration, and the full local stack remain in [`dfc-coder/system-g`](https://github.com/dfc-coder/system-g).

## Requirements

- Node.js 20.19 or newer
- npm
- Podman for the production-style container build

## Local development

```bash
npm install
npm run dev
```

The Vite development server listens on `http://localhost:5173` and proxies `/api` and `/health` to `http://localhost:8080` by default.

To target a separately hosted backend, create `.env.local`:

```env
VITE_API_BASE_URL=http://localhost:8080
```

## Production build

```bash
npm install
npm run build
```

## Podman

```bash
podman build -t localhost/growntrol-web:latest .
podman run --rm -p 5173:80 localhost/growntrol-web:latest
```

When the container joins the Growntrol Compose network, Nginx proxies `/api` and `/health` to the backend service named `backend`.

## Safety boundary

The dashboard publishes bounded command requests through the backend. It never controls GPIO directly. Firmware safety rules, irrigation interlocks, and the independent pump watchdog remain authoritative.
