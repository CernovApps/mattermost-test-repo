# Mattermost Team Edition

Open-source team collaboration and messaging platform, deployed via Coolify.

## Quick Start

### Prerequisites
- Coolify instance running
- Docker and Docker Compose

### Deployment via Coolify CLI

```bash
coolify app create dockercompose \
  --server-uuid <your-server-uuid> \
  --project-uuid <your-project-uuid> \
  --environment-name <environment-name> \
  --git-repository <your-username>/mattermost \
  --git-branch main \
  --build-pack dockercompose \
  --ports-exposes 8065 \
  --name "Mattermost" \
  --instant-deploy
```

### Local Deployment

```bash
# Copy environment file
cp .env.example .env

# Customize if needed (change DB_PASSWORD for production)
nano .env

# Start services
docker compose up -d

# Access Mattermost
# http://localhost:8065
```

## Services

- **Mattermost:** mattermost/mattermost-team-edition:9.9
- **PostgreSQL:** postgres:15-alpine

## Configuration

### Environment Variables

- `DB_PASSWORD` - PostgreSQL database password (default: SecurePassword123!)
- `MATTERMOST_URL` - Public URL for Mattermost (default: http://localhost:8065)

### After Deployment

1. Access Mattermost at `http://localhost:8065`
2. Complete the setup wizard
3. Configure SMTP for email notifications (optional)
4. Set up SSL/TLS via Coolify proxy

## Volumes

- `postgres_data` - PostgreSQL database files
- `mattermost_data` - User files and attachments
- `mattermost_config` - Mattermost configuration

## Health Checks

Both services have health checks enabled:
- PostgreSQL: `pg_isready` check every 10s
- Mattermost: HTTP ping to `/api/v4/system/ping` every 30s

## Logs

```bash
# View all logs
docker compose logs -f

# View Mattermost logs only
docker compose logs -f mattermost

# View PostgreSQL logs only
docker compose logs -f postgres
```

## Management

```bash
# Stop services
docker compose down

# Restart services
docker compose restart

# Update to latest version
# Edit docker-compose.yml and change the Mattermost image tag
docker compose pull
docker compose up -d
```

## Production Considerations

⚠️ **Before deploying to production:**

1. Change `DB_PASSWORD` in `.env` to a strong, random password
2. Set `MATTERMOST_URL` to your actual domain
3. Configure SSL/TLS certificates
4. Set up SMTP for email notifications
5. Configure backup strategy for `postgres_data` and `mattermost_data` volumes
6. Review Mattermost security settings in the admin panel
7. Set resource limits in docker-compose.yml if needed

## Support

- [Mattermost Documentation](https://docs.mattermost.com/)
- [Coolify Documentation](https://coolify.io/docs)
