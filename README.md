# OnlyOffice Docker Setup

This directory contains a complete OnlyOffice Document Server Docker setup with PostgreSQL database and pgAdmin for database management.

## Quick Start

1. Start all services:
   ```bash
   docker-compose up -d
   ```

2. Stop the services:
   ```bash
   docker-compose down
   ```

3. Stop and remove all data (careful!):
   ```bash
   docker-compose down -v
   ```

## Services Included

1. **OnlyOffice Document Server** - Document editing and collaboration
2. **PostgreSQL Database** - Database for advanced features
3. **pgAdmin** - Web interface for PostgreSQL management

## Connection Details

### OnlyOffice Document Server
- **URL**: http://localhost:8081
- **Status**: http://localhost:8081/health-check.json
- **No Authentication Required** (JWT disabled for local setup)

### PostgreSQL Database (for application use)
- **Host**: `localhost` (or `onlyoffice-postgres` from other Docker containers)
- **Port**: `5432`
- **Database**: `onlyoffice_db`
- **Username**: `onlyoffice_user`
- **Password**: `onlyoffice_password_789`

### pgAdmin (Web Interface)
- **URL**: http://localhost:5050
- **Email**: `admin@onlyoffice.local`
- **Password**: `pgadmin_password_123`

## Docker Network

The OnlyOffice containers are on the `onlyoffice-network` network. To connect other Docker containers to OnlyOffice:

1. Add your application containers to the same network in your docker-compose.yml:
   ```yaml
   networks:
     - onlyoffice-network

   networks:
     onlyoffice-network:
       external: true
   ```

2. Use `onlyoffice-document-server` as the hostname in your application's OnlyOffice server connection.

3. Use `onlyoffice-postgres` as the hostname for database connection from other containers.

## Files Structure

```
docker-onlyoffice/
├── docker-compose.yml      # Docker services configuration
├── .env                    # Environment variables (credentials)
└── README.md              # This file
```

## Security Notes

- Change the passwords in `.env` file before production use
- Enable JWT authentication in docker-compose.yml for production
- The current setup allows connections from any host (%) - restrict this in production
- Consider using Docker secrets for sensitive data in production

## Connecting from Other Docker Containers

When connecting from other Docker containers:

### OnlyOffice Document Server:
```
http://onlyoffice-document-server/
```

### PostgreSQL Database:
```
postgresql://onlyoffice_user:onlyoffice_password_789@onlyoffice-postgres:5432/onlyoffice_db
```

## Useful Commands

### View logs
```bash
# OnlyOffice Document Server logs
docker logs onlyoffice-server

# PostgreSQL logs
docker logs onlyoffice-postgres

# pgAdmin logs
docker logs onlyoffice-pgadmin
```

### Check container status
```bash
docker ps
```

### Access container shell
```bash
# OnlyOffice container
docker exec -it onlyoffice-server bash

# PostgreSQL container
docker exec -it onlyoffice-postgres bash
```

## Testing OnlyOffice

1. Open http://localhost:8081 in your browser
2. You should see the OnlyOffice Document Server landing page
3. The server is ready for document editing and collaboration

## Troubleshooting

- If port 8081 is already in use, change it in `docker-compose.yml` and `.env`
- If containers fail to start, check logs with `docker logs <container_name>`
- Make sure Docker is running before executing docker-compose commands
- For Windows WSL2, ensure volumes are created in a WSL-compatible directory

## Additional Features

To enable JWT authentication for production, modify the docker-compose.yml:
```yaml
environment:
  JWT_ENABLED: "true"
  JWT_SECRET: "your-secret-key"
```

## References

- [OnlyOffice Documentation](https://helpcenter.onlyoffice.com/)
- [OnlyOffice Docker Documentation](https://hub.docker.com/r/onlyoffice/documentserver)
- [PostgreSQL Documentation](https://www.postgresql.org/docs/)
