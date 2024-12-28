# OpenTelemetry Demo - Practical Implementation Guide

## What is OpenTelemetry?

OpenTelemetry is a tool that helps developers see what's happening inside their applications. Imagine it as a magnifying glass that lets you look at the inner workings of a program to understand how it behaves and performs.

## What is this Project About?

This project is a demo (a demonstration) of how OpenTelemetry works. It uses a pretend online store to show how you can track and monitor different parts of an application. This helps developers find and fix problems more easily.


## Introduction
This guide will help you understand and implement OpenTelemetry using a microservices-based e-commerce application. The project demonstrates real-world monitoring and tracing in a distributed system.

## Prerequisites
- Docker Desktop installed
- Minimum 8GB RAM
- Git installed
- Basic understanding of terminal/command line

## System Requirements
- CPU: 2+ cores
- Memory: 8GB+ RAM
- Disk: 20GB+ free space
- Ports: 8080, 4317, 4318, 6379, 50051 should be available

## Quick Start

### 1. Clone the Repository
```bash
git clone https://github.com/iemafzalhassan/opentelemetry-demo.git
cd opentelemetry-demo
```

### 2. Start the Application
```bash
# Build and start services
docker compose -f docker-compose.yml build
docker compose -f docker-compose.yml up -d
```

### 3. Access Services
- Frontend (Web Store): http://localhost:8080
- OpenTelemetry Collector: http://localhost:4317 (gRPC), http://localhost:4318 (HTTP)

## Core Services Explained

### Frontend Layer
1. **Frontend Service**
   - Main web interface
   - Port: 8080
   - Memory Limit: 250MB
   - Dependencies: cart, checkout, product catalog, shipping

2. **Frontend Proxy**
   - Handles routing and load balancing
   - Port: 8080
   - Memory Limit: 50MB

### Business Services
1. **Cart Service**
   - Manages shopping cart
   - Memory Limit: 160MB
   - Depends on Redis

2. **Checkout Service**
   - Handles order processing
   - Memory Limit: 20MB
   - Dependencies: cart, email, payment, shipping

3. **Shipping Service**
   - Manages shipping calculations
   - Port: 50051
   - Dependencies: quote service

4. **Quote Service**
   - Calculates shipping quotes
   - Port: 8090
   - Dependencies: OpenTelemetry collector

### Support Services
1. **Currency Service**
   - Handles currency conversions
   - Memory Limit: 20MB

2. **Email Service**
   - Manages email notifications
   - Memory Limit: 100MB

3. **Payment Service**
   - Processes payments
   - Memory Limit: 120MB

### Infrastructure Services
1. **OpenTelemetry Collector**
   - Collects and processes telemetry data
   - Ports: 4317 (gRPC), 4318 (HTTP)

2. **Redis (Valkey)**
   - Cache and session storage
   - Port: 6379

## Monitoring Setup

### OpenTelemetry Configuration
1. Environment Variables:
```yaml
- OTEL_SERVICE_NAME=<service-name>
- OTEL_EXPORTER_OTLP_ENDPOINT=http://otel-collector:4317
- OTEL_TRACES_SAMPLER=always_on
- OTEL_METRICS_EXPORTER=otlp
- OTEL_LOGS_EXPORTER=otlp
```

### Logging Configuration
All services use JSON file logging with:
- Maximum size: 5MB
- Maximum files: 2
- Tagged with container name

## Troubleshooting Guide

### Common Issues

1. **Service Won't Start**
```bash
# Check service logs
docker compose -f docker-compose.yml logs <service-name>
```

2. **Memory Issues**
```bash
# Check memory usage
docker stats

# Adjust memory limits in docker-compose.yml if needed
```

3. **Port Conflicts**
```bash
# Check port usage
netstat -tulpn | grep <port-number>

# Change ports in docker-compose.yml if needed
```

### Maintenance Commands

1. **Restart Specific Service**
```bash
docker compose -f docker-compose.yml restart <service-name>
```

2. **View Service Logs**
```bash
docker compose -f docker-compose.yml logs -f <service-name>
```

3. **Clean Up**
```bash
# Stop all services
docker compose -f docker-compose.yml down

# Remove volumes
docker volume prune

# Remove unused images
docker image prune
```

## Support and Resources
- GitHub Issues: [Project Issues](https://github.com/iemafzalhassan/opentelemetry-demo/issues)
- OpenTelemetry Documentation: [Official Docs](https://opentelemetry.io/docs/)
- Docker Documentation: [Docker Docs](https://docs.docker.com/)

## Contributing
Feel free to contribute by:
1. Forking the repository
2. Creating a feature branch
3. Submitting a pull request
