# WordPress Composer Kubernetes Setup

This repository contains a WordPress setup using Composer for dependency management, Docker for local development, and Kubernetes for production deployment.

## Prerequisites

1. Install required software:
   - [Docker Desktop](https://www.docker.com/products/docker-desktop)
   - [Git](https://git-scm.com/downloads)

That's it! No need for PHP or Composer locally - everything is handled via Docker.

## Initial Setup

1. Clone the repository:
```bash
git clone [repository-url]
cd wordpress-composer-k8s
```

2. Create the required directory structure:
```bash
mkdir -p web/app/plugins
mkdir -p config/nginx
```

3. Start the local environment:
```bash
docker-compose -f docker-compose.override.yml up -d
```

4. Access WordPress at `http://localhost:8080`

## How it works

- The Dockerfile uses a multi-stage build:
  1. First stage uses the Composer image to install dependencies
  2. Second stage uses WordPress PHP-FPM image
  3. All PHP and Composer operations happen inside Docker containers
- No need to install PHP or Composer locally
- All dependencies are managed within Docker

## Production Deployment

### Option 1: Manual Deployment

1. Ensure you have access to a Kubernetes cluster
2. Deploy using Helm:
```bash
helm install wordpress ./helm/wordpress
```

### Option 2: Automated Deployment

1. Push your code to GitHub
2. The GitHub Actions workflow will automatically:
   - Build a Docker image
   - Push to Docker Hub
   - Deploy to your Kubernetes cluster

## Project Structure

```
wordpress-composer-k8s/
├── Dockerfile
├── docker-compose.override.yml (voor local)
├── composer.json
├── composer.lock
├── config/
│   └── nginx/
├── .env.example
├── web/
│   └── app/
│       └── plugins/ (leeg, of met GF als zip install)
├── helm/
│   └── wordpress/
│       ├── Chart.yaml
│       ├── values.yaml
│       └── templates/
│           ├── deployment.yaml
│           ├── pvc.yaml
├── .github/
│   └── workflows/
│       └── deploy.yaml
```

## Requirements

- Docker and Docker Compose for local development
- Kubernetes cluster for production
- Helm for Kubernetes deployment
- GitHub account for automated deployments
