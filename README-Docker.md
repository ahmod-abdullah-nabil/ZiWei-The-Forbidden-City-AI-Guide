# Docker Setup for ZiWei - The Forbidden City AI Guide

## Architecture

```
[ Browser ]
   ?
http://localhost
   ?
[ Docker Container ]
   ?? Nginx (serves website on port 80)
   ?? React build files
   ?? 3D models (.glb)
   ?? Local assets
```

## Prerequisites

- Docker Desktop installed
- Docker Compose installed (usually comes with Docker Desktop)

## Quick Start

### 1. Build and Run

```bash
# Build and start the container
docker-compose up --build

# Or run in detached mode (background)
docker-compose up -d --build
```

### 2. Access Your Website

Open your browser and navigate to:
```
http://localhost
```

### 3. Stop the Container

```bash
# Stop the container
docker-compose down

# Stop and remove volumes
docker-compose down -v
```

## Project Structure

Make sure your project has the following structure:

```
ZiWei-The-Forbidden-City-AI-Guide/
??? public/               # Static assets
?   ??? models/          # 3D .glb files
?   ??? images/          # Images
?   ??? index.html       # HTML template
??? src/                 # React source code
?   ??? App.jsx          # Main App component
?   ??? main.jsx         # Entry point
?   ??? ...              # Other components
??? package.json         # Dependencies
??? vite.config.js       # Vite config (or webpack/CRA config)
??? Dockerfile           # Docker build instructions
??? docker-compose.yml   # Docker orchestration
??? nginx.conf           # Nginx server configuration
??? .dockerignore        # Files to exclude from Docker build
```

## Development vs Production

### Development (Hot Reload)
For development with hot reloading, you can run:
```bash
npm install
npm run dev
```

### Production (Docker)
For production deployment:
```bash
docker-compose up -d --build
```

## Customization

### Change Port
Edit `docker-compose.yml` to change the port:
```yaml
ports:
  - "8080:80"  # Access via http://localhost:8080
```

### Live Update 3D Models
Uncomment volume mounts in `docker-compose.yml`:
```yaml
volumes:
  - ./public/models:/usr/share/nginx/html/models
```

This allows you to update models without rebuilding the container.

## Useful Commands

```bash
# View logs
docker-compose logs -f

# Rebuild without cache
docker-compose build --no-cache

# List running containers
docker ps

# Execute command inside container
docker exec -it ziwei-forbidden-city sh

# Check nginx configuration
docker exec ziwei-forbidden-city nginx -t
```

## Troubleshooting

### Container won't start
- Check if port 80 is already in use
- View logs: `docker-compose logs`

### 3D models not loading
- Verify files are in `public/models/` directory
- Check browser console for CORS errors
- Ensure nginx.conf includes proper MIME types for .glb files

### Build fails
- Ensure `package.json` exists
- Check that build script is defined: `"build": "vite build"` or similar
- Verify Node.js version compatibility

## Health Check

Visit `http://localhost/health` to check if Nginx is running properly.
