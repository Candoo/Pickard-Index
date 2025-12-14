# Thornton Pickard Full-Stack Application

A comprehensive web application for cataloging and managing Thornton Pickard cameras and ephemera. This deployment repository orchestrates the full stack comprising a React frontend, Go API backend, and PostgreSQL database.

## 🎯 About

The Thornton Pickard application is designed for camera collectors, historians, and enthusiasts to catalog, search, and manage information about Thornton Pickard cameras and related ephemera. The application features:

- **Searchable Camera Database** - Browse and search through historical camera models
- **User Authentication** - Secure JWT-based authentication system
- **Image Uploads** - Upload and manage camera images
- **Advanced Filtering** - Filter by manufacturer, year, format, and more
- **RESTful API** - Well-documented API for integration and development

## 🏗️ Architecture

This full-stack application consists of three separate repositories:

1. **[Thornton-Pickard-Api](https://github.com/Candoo/Thornton-Pickard-Api)** - Go/Gin REST API backend
2. **[Thornton-Pickard-Ui](https://github.com/Candoo/Thornton-Pickard-Ui)** - React 19 frontend with TypeScript
3. **Thornton-Pickard-Deployment** (this repo) - Docker Compose deployment configuration

```
┌─────────────────────────────────────────────────────┐
│                                                     │
│  React Frontend (Thornton-Pickard-Ui)             │
│  Port: 3000                                         │
│                                                     │
└──────────────────┬──────────────────────────────────┘
                   │
                   │ HTTP/REST
                   │
┌──────────────────▼──────────────────────────────────┐
│                                                     │
│  Go/Gin API (Thornton-Pickard-Api)                  │
│  Port: 8080                                         │
│                                                     │
└──────────────────┬──────────────────────────────────┘
                   │
                   │ SQL
                   │
┌──────────────────▼──────────────────────────────────┐
│                                                     │
│  PostgreSQL Database                                │
│  Port: 5432                                         │
│                                                     │
└─────────────────────────────────────────────────────┘
```

## ⚙️ Quick Start Setup

Follow these steps to get the entire application running on your local machine.

### Prerequisites

- Docker Desktop or Docker Engine with Docker Compose
- Git

### Installation Steps

1. **Create a central workspace directory** (e.g., `Projects`) and navigate into it:

   ```bash
   mkdir Projects
   cd Projects
   ```

2. **Clone all three repositories into this central workspace:**

   ```bash
   git clone https://github.com/Candoo/Thornton-Pickard-Deployment.git
   git clone https://github.com/Candoo/Thornton-Pickard-Api.git
   git clone https://github.com/Candoo/Thornton-Pickard-Ui.git
   ```

   **Your resulting directory structure MUST look like this:**

   ```
   Projects/
   ├── Thornton-Pickard-Api/
   ├── Thornton-Pickard-Ui/
   └── Thornton-Pickard-Deployment/          <-- Deployment configuration
   ```

3. **Navigate into the deployment directory:**

   ```bash
   cd Thornton-Pickard-Deployment
   ```

4. **Configure environment variables:**

   ```bash
   cp .env.example .env
   ```

   **⚠️ IMPORTANT:** Review and edit the `.env` file to change default credentials:

   ```env
   # Database Configuration
   POSTGRES_USER=postgres
   POSTGRES_PASSWORD=your-secure-password  # Change this!
   POSTGRES_DB=thornton_pickard
   
   # API Configuration
   JWT_SECRET=your-jwt-secret-key          # Change this!
   SEED=true
   
   # Backend API URL (for frontend)
   VITE_API_URL=http://localhost:8080
   ```

5. **Launch the full stack:**

   ```bash
   docker compose up -d --build
   ```

   This will:
   - Build and start the PostgreSQL database
   - Build and start the Go API backend
   - Build and start the React frontend
   - Create necessary volumes for data persistence

6. **Verify services are running:**

   ```bash
   docker compose ps
   ```

   All services should show as "Up" or "healthy".

## 🚀 Accessing the Application

Once running, you can access:

- **Frontend Application:** http://localhost:3000
- **API Backend:** http://localhost:8080
- **API Documentation (Swagger):** http://localhost:8080/swagger/index.html
- **Health Check:** http://localhost:8080/health

### Default Credentials

After seeding, use these default credentials to log in:

- **Email:** `admin@thorntonpickard.com`
- **Password:** `admin123`

**⚠️ Change these credentials immediately in production environments!**

## 🛠️ Docker Commands

### Viewing Logs

```bash
# View all logs
docker compose logs -f

# View specific service logs
docker compose logs -f frontend
docker compose logs -f api
docker compose logs -f db
```

### Managing Services

```bash
# Stop all services
docker compose down

# Stop and remove volumes (⚠️ deletes all data)
docker compose down -v

# Restart a specific service
docker compose restart api

# Rebuild and restart after code changes
docker compose up -d --build
```

### Database Operations

```bash
# Access PostgreSQL shell
docker compose exec db psql -U postgres -d thornton_pickard

# Backup database
docker compose exec db pg_dump -U postgres thornton_pickard > backup.sql

# Restore database
docker compose exec -T db psql -U postgres thornton_pickard < backup.sql
```

## 📁 Project Structure

```
Pickard-Index/
├── docker-compose.yml      # Main orchestration file
├── .env.example            # Environment template
├── .env                    # Your environment (git-ignored)
├── .gitignore
└── README.md               # This file
```

## 🔧 Development

### Working on Individual Services

Each repository has its own README with development instructions:

- **Frontend Development:** See [Thornton-Pickard-Ui/README.md](https://github.com/Candoo/Thornton-Pickard-Ui)
- **Backend Development:** See [Thornton-Pickard-Api/README.md](https://github.com/Candoo/Thornton-Pickard-Api)

### Running Services Individually

You can also run services outside of Docker for development:

1. **Backend only:**
   ```bash
   cd ../Thornton-Pickard-Api
   # Follow backend README for local setup
   ```

2. **Frontend only:**
   ```bash
   cd ../Thornton-Pickard-Ui
   # Follow frontend README for local setup
   ```

## 🐛 Troubleshooting

### Port Conflicts

If you get port conflict errors:

```bash
# Check what's using the ports
lsof -i :3000  # Frontend
lsof -i :8080  # Backend
lsof -i :5432  # Database

# Stop conflicting services or change ports in docker-compose.yml
```

### Database Connection Issues

```bash
# Check database is running
docker compose ps db

# View database logs
docker compose logs db

# Ensure database is initialized
docker compose exec db psql -U postgres -l
```

### Frontend Can't Connect to API

1. Check the API is running: `curl http://localhost:8080/health`
2. Verify `VITE_API_URL` in `.env` is correct
3. Check browser console for CORS errors
4. Rebuild frontend: `docker compose up -d --build frontend`

### Clearing Everything and Starting Fresh

```bash
# Stop and remove all containers, networks, and volumes
docker compose down -v

# Remove any orphaned containers
docker compose down --remove-orphans

# Start fresh
docker compose up -d --build
```

## 🔒 Security Considerations

Before deploying to production:

1. **Change all default passwords** in `.env`
2. **Generate a strong JWT secret** (at least 32 characters)
3. **Use HTTPS** with proper SSL/TLS certificates
4. **Configure CORS** properly in the API
5. **Set up proper database backups**
6. **Review and harden Docker security settings**
7. **Implement rate limiting** on the API
8. **Enable authentication** on all protected endpoints

## 📊 Data Persistence

Data is persisted in Docker volumes:

- **postgres_data** - Database data
- **api_uploads** - Uploaded images

To backup volumes:

```bash
# List volumes
docker volume ls

# Backup a volume
docker run --rm -v pickard-index_postgres_data:/data -v $(pwd):/backup ubuntu tar czf /backup/postgres_backup.tar.gz /data
```

## 🚢 Deployment

This Docker Compose setup is suitable for:

- Local development
- Staging environments
- Small-scale production deployments

For larger production deployments, consider:

- **Kubernetes** for orchestration
- **Managed databases** (AWS RDS, Google Cloud SQL)
- **Container registries** (Docker Hub, AWS ECR)
- **Load balancers** for the frontend and API
- **CDN** for static assets

## 📄 License

MIT License - see individual repositories for details.

## 🆘 Support

For issues related to:

- **Frontend:** Open issue in [Thornton-Pickard-Ui](https://github.com/Candoo/Thornton-Pickard-Ui/issues)
- **Backend:** Open issue in [Thornton-Pickard-Api](https://github.com/Candoo/Thornton-Pickard-Api/issues)
- **Deployment:** Open issue in this repository

## 🔗 Related Links

- [Frontend Repository](https://github.com/Candoo/Thornton-Pickard-Ui)
- [Backend API Repository](https://github.com/Candoo/Thornton-Pickard-Api)
- [API Documentation](http://localhost:8080/swagger/index.html) (when running)

---

**Built with ❤️ for camera enthusiasts and collectors**
