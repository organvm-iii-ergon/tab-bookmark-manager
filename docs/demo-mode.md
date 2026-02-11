# Demo Mode

Run the full Tab Bookmark Manager stack locally with seed data.

## Quick Start

```bash
# 1. Clone and enter
git clone https://github.com/organvm-iii-ergon/tab-bookmark-manager.git
cd tab-bookmark-manager

# 2. Copy environment files
cp backend/.env.example backend/.env
cp ml-service/.env.example ml-service/.env

# 3. Start all services
docker-compose up -d

# 4. Wait for healthy status
docker-compose ps
# All services should show "Up (healthy)"

# 5. Seed demo data
curl -X POST http://localhost:3000/api/auth/register \
  -H "Content-Type: application/json" \
  -d '{"email":"demo@example.com","password":"demo1234","name":"Demo User"}'

# 6. Login and get token
TOKEN=$(curl -s -X POST http://localhost:3000/api/auth/login \
  -H "Content-Type: application/json" \
  -d '{"email":"demo@example.com","password":"demo1234"}' | jq -r .token)

# 7. Test bookmark creation
curl -X POST http://localhost:3000/api/bookmarks \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $TOKEN" \
  -d '{"url":"https://example.com","title":"Example Site","tags":["demo"]}'

# 8. Test ML categorization
curl -X POST http://localhost:5000/api/analyze \
  -H "Content-Type: application/json" \
  -d '{"text":"Machine learning and artificial intelligence research","url":"https://arxiv.org"}'
```

## Service Endpoints

| Service | URL | Health Check |
|---------|-----|------|
| Backend API | http://localhost:3000 | GET /health |
| ML Service | http://localhost:5000 | GET /health |
| Swagger Docs | http://localhost:3000/api-docs | — |
| PostgreSQL | localhost:5432 | — |
| Redis | localhost:6379 | — |

## Browser Extension

1. Open Chrome → `chrome://extensions`
2. Enable "Developer mode"
3. Click "Load unpacked" → select the `extension/` directory
4. The extension icon appears in the toolbar

## Teardown

```bash
docker-compose down        # Stop services
docker-compose down -v     # Stop and remove volumes
```

