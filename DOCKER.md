# Docker Setup Guide

## Overview
This guide explains how to build and run the Node.js application using Docker.

## Prerequisites
- [Docker Desktop](https://www.docker.com/products/docker-desktop) installed
- For GPU support: [NVIDIA Container Toolkit](https://github.com/NVIDIA/nvidia-docker)

## Quick Start

### Option 1: Using Docker Compose (Recommended)
```bash
# Build and start the application
docker-compose up --build

# Run in background
docker-compose up -d --build

# Stop the application
docker-compose down

# View logs
docker-compose logs -f
```

### Option 2: Using Docker CLI
```bash
# Build the image
docker build -t wad-assignment2a .

# Run the container
docker run -p 3000:3000 wad-assignment2a

# Run in background
docker run -d -p 3000:3000 --name my-app wad-assignment2a

# Stop the container
docker stop my-app

# View logs
docker logs -f my-app
```

## File Structure
- **Dockerfile** - Base image and application setup
- **.dockerignore** - Files excluded from Docker build
- **docker-compose.yml** - Multi-container orchestration
- **package.json** - Node.js dependencies
- **server.js** - Application entry point

## Environment Variables
Set in `docker-compose.yml`:
- `NODE_ENV=production` - Production environment

## Port Mapping
- Container Port: 3000
- Host Port: 3000 (configurable in docker-compose.yml)

Access at: `http://localhost:3000`

## Useful Docker Commands

### Image Commands
```bash
# List images
docker images

# Remove image
docker rmi wad-assignment2a

# Build with custom tag
docker build -t wad-assignment2a:v1.0 .
```

### Container Commands
```bash
# List running containers
docker ps

# List all containers
docker ps -a

# Execute command in running container
docker exec -it my-app sh

# Remove container
docker rm my-app
```

### Compose Commands
```bash
# Build services
docker-compose build

# Start services
docker-compose up

# Stop services
docker-compose stop

# Restart services
docker-compose restart

# Remove containers
docker-compose down -v  # with volume removal
```

## GPU Support (Optional)
For GPU-enabled applications using NVIDIA Container Toolkit:

1. Install [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html)

2. Update `docker-compose.yml`:
```yaml
services:
  web:
    build: .
    runtime: nvidia
    environment:
      - NVIDIA_VISIBLE_DEVICES=all
```

3. Run with GPU:
```bash
docker-compose up
```

## Troubleshooting

### Port already in use
Change port mapping in `docker-compose.yml`:
```yaml
ports:
  - "8080:3000"  # Access at localhost:8080
```

### Build fails
```bash
# Clean build
docker-compose build --no-cache

# View build logs
docker-compose build --verbose
```

### Container exits immediately
```bash
# Check logs
docker-compose logs

# Run interactively
docker-compose run web sh
```

### Permission denied
On Linux, add your user to docker group:
```bash
sudo usermod -aG docker $USER
newgrp docker
```

## Production Deployment
For production, consider:
- Use specific image versions (not `latest`)
- Set resource limits in docker-compose.yml
- Use `.env` file for secrets
- Enable health checks
- Configure logging drivers

## Additional Resources
- [Docker Documentation](https://docs.docker.com/)
- [Docker Compose Reference](https://docs.docker.com/compose/compose-file/)
- [Node.js Docker Best Practices](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
