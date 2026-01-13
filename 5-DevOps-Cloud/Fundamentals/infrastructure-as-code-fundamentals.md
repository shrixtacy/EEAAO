# Infrastructure as Code Fundamentals

## Overview

Infrastructure as Code (IaC) treats your infrastructure like software - versioned, tested, and automated. Instead of clicking through cloud consoles or running manual commands, you write code that describes your infrastructure and let tools create it for you.

**Why This Matters:**
- Reproducible environments (no more "works on my machine" for infrastructure)
- Version control for infrastructure changes
- Automated testing and validation
- Disaster recovery through code recreation
- Team collaboration on infrastructure

**What Problems IaC Solves:**
- Manual configuration drift between environments
- Inability to recreate infrastructure quickly
- Lack of infrastructure change history
- Human errors in manual deployments
- Difficulty scaling infrastructure management

## Docker Fundamentals

### What Docker Solves

**The Problem:** "It works on my machine" - applications behave differently across environments due to different operating systems, dependencies, and configurations.

**The Solution:** Containers package your application with all its dependencies, ensuring consistent behavior everywhere.

### Core Docker Concepts

**1. Images vs Containers**
```bash
# An image is a blueprint, a container is a running instance
docker build -t my-app .          # Create image from Dockerfile
docker run my-app                 # Run container from image
docker ps                         # List running containers
docker images                     # List available images
```

**2. Dockerfile Basics**
```dockerfile
# Dockerfile - defines how to build your application image
FROM node:18-alpine              # Start with Node.js base image
WORKDIR /app                     # Set working directory
COPY package*.json ./            # Copy dependency files
RUN npm install                  # Install dependencies
COPY . .                         # Copy application code
EXPOSE 3000                      # Document which port app uses
CMD ["npm", "start"]             # Command to run when container starts
```

**3. Building and Running**
```bash
# Build your application image
docker build -t my-web-app .

# Run your container
docker run -p 3000:3000 my-web-app

# Run in background (detached mode)
docker run -d -p 3000:3000 --name my-app my-web-app

# View logs
docker logs my-app

# Stop container
docker stop my-app
```

### Docker Compose for Multi-Service Applications

**The Problem:** Real applications need databases, caches, and other services. Managing multiple containers manually becomes complex.

**The Solution:** Docker Compose defines multi-container applications in a single file.

```yaml
# docker-compose.yml
version: '3.8'
services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - DATABASE_URL=postgresql://user:password@db:5432/myapp
    depends_on:
      - db
      - redis

  db:
    image: postgres:15
    environment:
      - POSTGRES_USER=user
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=myapp
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    ports:
      - "6379:6379"

volumes:
  postgres_data:
```

```bash
# Start all services
docker-compose up

# Start in background
docker-compose up -d

# View logs for all services
docker-compose logs

# Stop all services
docker-compose down

# Rebuild and restart
docker-compose up --build
```

### Container Best Practices

**1. Keep Images Small**
```dockerfile
# Use specific, minimal base images
FROM node:18-alpine              # Alpine is smaller than full Ubuntu

# Use multi-stage builds for compiled languages
FROM golang:1.19 AS builder
WORKDIR /app
COPY . .
RUN go build -o main .

FROM alpine:latest
RUN apk --no-cache add ca-certificates
COPY --from=builder /app/main .
CMD ["./main"]
```

**2. Handle Secrets Properly**
```bash
# DON'T put secrets in Dockerfile or images
# DO use environment variables or secret management
docker run -e DATABASE_PASSWORD=secret my-app

# Or use Docker secrets (in production)
echo "my-secret" | docker secret create db_password -
```

**3. Use .dockerignore**
```
# .dockerignore - exclude files from build context
node_modules
.git
.env
*.log
README.md
```

## Terraform Fundamentals

### What Terraform Solves

**The Problem:** Cloud resources are created through web consoles or CLI commands, making them hard to reproduce, version, or share with teams.

**The Solution:** Terraform uses declarative configuration files to define infrastructure, then creates and manages those resources automatically.

### Core Terraform Concepts

**1. Providers and Resources**
```hcl
# main.tf - Terraform configuration file
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

# Configure the AWS Provider
provider "aws" {
  region = "us-west-2"
}

# Create a VPC
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name = "main-vpc"
  }
}

# Create a subnet
resource "aws_subnet" "public" {
  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "us-west-2a"
  map_public_ip_on_launch = true

  tags = {
    Name = "public-subnet"
  }
}
```

**2. Basic Terraform Workflow**
```bash
# Initialize Terraform (download providers)
terraform init

# See what Terraform will create
terraform plan

# Apply the changes
terraform apply

# View current state
terraform show

# Destroy resources
terraform destroy
```

### AWS Infrastructure Example

**Complete Web Application Infrastructure:**
```hcl
# variables.tf - Define input variables
variable "app_name" {
  description = "Name of the application"
  type        = string
  default     = "my-web-app"
}

variable "environment" {
  description = "Environment (dev, staging, prod)"
  type        = string
  default     = "dev"
}

# main.tf - Main infrastructure
# VPC and Networking
resource "aws_vpc" "main" {
  cidr_block           = "10.0.0.0/16"
  enable_dns_hostnames = true
  enable_dns_support   = true

  tags = {
    Name        = "${var.app_name}-vpc"
    Environment = var.environment
  }
}

resource "aws_internet_gateway" "main" {
  vpc_id = aws_vpc.main.id

  tags = {
    Name = "${var.app_name}-igw"
  }
}

resource "aws_subnet" "public" {
  count = 2

  vpc_id                  = aws_vpc.main.id
  cidr_block              = "10.0.${count.index + 1}.0/24"
  availability_zone       = data.aws_availability_zones.available.names[count.index]
  map_public_ip_on_launch = true

  tags = {
    Name = "${var.app_name}-public-${count.index + 1}"
  }
}

# Security Group
resource "aws_security_group" "web" {
  name_prefix = "${var.app_name}-web"
  vpc_id      = aws_vpc.main.id

  ingress {
    from_port   = 80
    to_port     = 80
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  ingress {
    from_port   = 443
    to_port     = 443
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }

  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }

  tags = {
    Name = "${var.app_name}-web-sg"
  }
}

# Application Load Balancer
resource "aws_lb" "main" {
  name               = "${var.app_name}-alb"
  internal           = false
  load_balancer_type = "application"
  security_groups    = [aws_security_group.web.id]
  subnets            = aws_subnet.public[*].id

  tags = {
    Name = "${var.app_name}-alb"
  }
}

# ECS Cluster for containers
resource "aws_ecs_cluster" "main" {
  name = "${var.app_name}-cluster"

  tags = {
    Name = "${var.app_name}-cluster"
  }
}
```

### Azure Infrastructure Example

**Resource Group and App Service:**
```hcl
# Configure Azure Provider
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }
}

provider "azurerm" {
  features {}
}

# Resource Group
resource "azurerm_resource_group" "main" {
  name     = "${var.app_name}-rg"
  location = "East US"

  tags = {
    Environment = var.environment
  }
}

# App Service Plan
resource "azurerm_service_plan" "main" {
  name                = "${var.app_name}-plan"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  os_type             = "Linux"
  sku_name            = "B1"
}

# App Service
resource "azurerm_linux_web_app" "main" {
  name                = "${var.app_name}-app"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_service_plan.main.location
  service_plan_id     = azurerm_service_plan.main.id

  site_config {
    application_stack {
      node_version = "18-lts"
    }
  }

  app_settings = {
    "ENVIRONMENT" = var.environment
  }
}

# Azure Database for PostgreSQL
resource "azurerm_postgresql_flexible_server" "main" {
  name                   = "${var.app_name}-db"
  resource_group_name    = azurerm_resource_group.main.name
  location               = azurerm_resource_group.main.location
  version                = "13"
  administrator_login    = "adminuser"
  administrator_password = var.db_password
  storage_mb             = 32768
  sku_name               = "B_Standard_B1ms"
}
```

### Terraform Best Practices

**1. State Management**
```hcl
# backend.tf - Remote state storage
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "infrastructure/terraform.tfstate"
    region = "us-west-2"
  }
}
```

**2. Variable Organization**
```hcl
# terraform.tfvars - Environment-specific values
app_name    = "my-production-app"
environment = "prod"
db_password = "secure-password-from-env"

# Use environment variables for secrets
# export TF_VAR_db_password="secure-password"
```

**3. Output Values**
```hcl
# outputs.tf - Export important values
output "load_balancer_dns" {
  description = "DNS name of the load balancer"
  value       = aws_lb.main.dns_name
}

output "database_endpoint" {
  description = "Database connection endpoint"
  value       = aws_db_instance.main.endpoint
  sensitive   = true
}
```

## Kubernetes Fundamentals

### When You Actually Need Kubernetes

**DON'T Use Kubernetes If:**
- You have a single application with simple scaling needs
- Your team is small (< 5 developers) and new to containers
- You're just getting started with containerization
- Simple container hosting (like AWS ECS, Azure Container Instances) meets your needs

**DO Use Kubernetes When:**
- You have multiple microservices that need orchestration
- You need advanced deployment strategies (canary, blue-green)
- You require complex networking between services
- You need portable container orchestration across cloud providers
- Your team has container experience and needs advanced features

### Core Kubernetes Concepts

**1. Pods and Deployments**
```yaml
# deployment.yaml - Defines how to run your application
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
      - name: web
        image: my-web-app:latest
        ports:
        - containerPort: 3000
        env:
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-secret
              key: url
        resources:
          requests:
            memory: "128Mi"
            cpu: "100m"
          limits:
            memory: "256Mi"
            cpu: "200m"
```

**2. Services and Networking**
```yaml
# service.yaml - Exposes your application
apiVersion: v1
kind: Service
metadata:
  name: web-app-service
spec:
  selector:
    app: web-app
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```

**3. ConfigMaps and Secrets**
```yaml
# configmap.yaml - Configuration data
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  environment: "production"
  log_level: "info"
  
---
# secret.yaml - Sensitive data
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
type: Opaque
data:
  url: cG9zdGdyZXNxbDovL3VzZXI6cGFzc0BkYjozNTQzMi9hcHA=  # base64 encoded
```

**4. Basic kubectl Commands**
```bash
# Apply configuration
kubectl apply -f deployment.yaml

# View resources
kubectl get pods
kubectl get services
kubectl get deployments

# View logs
kubectl logs -f deployment/web-app

# Scale application
kubectl scale deployment web-app --replicas=5

# Update image
kubectl set image deployment/web-app web=my-web-app:v2

# Port forward for local testing
kubectl port-forward service/web-app-service 8080:80
```

### Kubernetes with Terraform

**Managing Kubernetes Resources with Terraform:**
```hcl
# kubernetes.tf - Kubernetes resources via Terraform
resource "kubernetes_deployment" "web_app" {
  metadata {
    name = "web-app"
    labels = {
      app = "web-app"
    }
  }

  spec {
    replicas = 3

    selector {
      match_labels = {
        app = "web-app"
      }
    }

    template {
      metadata {
        labels = {
          app = "web-app"
        }
      }

      spec {
        container {
          image = "my-web-app:latest"
          name  = "web"

          port {
            container_port = 3000
          }

          env {
            name = "DATABASE_URL"
            value_from {
              secret_key_ref {
                name = kubernetes_secret.db_secret.metadata[0].name
                key  = "url"
              }
            }
          }

          resources {
            limits = {
              cpu    = "200m"
              memory = "256Mi"
            }
            requests = {
              cpu    = "100m"
              memory = "128Mi"
            }
          }
        }
      }
    }
  }
}

resource "kubernetes_service" "web_app" {
  metadata {
    name = "web-app-service"
  }

  spec {
    selector = {
      app = kubernetes_deployment.web_app.metadata[0].labels.app
    }

    port {
      port        = 80
      target_port = 3000
    }

    type = "LoadBalancer"
  }
}
```

## Infrastructure Monitoring and Logging

### Why Monitoring Matters

**The Problem:** You can't fix what you can't see. Without monitoring, you're flying blind when issues occur.

**What to Monitor:**
- Application health and performance
- Infrastructure resource usage
- User experience metrics
- Security events and anomalies

### Basic Monitoring Setup

**1. Application Health Checks**
```yaml
# In your Kubernetes deployment
livenessProbe:
  httpGet:
    path: /health
    port: 3000
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /ready
    port: 3000
  initialDelaySeconds: 5
  periodSeconds: 5
```

**2. Prometheus Configuration**
```yaml
# prometheus.yml - Monitoring configuration
global:
  scrape_interval: 15s

scrape_configs:
  - job_name: 'kubernetes-pods'
    kubernetes_sd_configs:
      - role: pod
    relabel_configs:
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_scrape]
        action: keep
        regex: true
      - source_labels: [__meta_kubernetes_pod_annotation_prometheus_io_path]
        action: replace
        target_label: __metrics_path__
        regex: (.+)
```

**3. Grafana Dashboard Setup**
```json
{
  "dashboard": {
    "title": "Application Metrics",
    "panels": [
      {
        "title": "Request Rate",
        "type": "graph",
        "targets": [
          {
            "expr": "rate(http_requests_total[5m])",
            "legendFormat": "{{method}} {{status}}"
          }
        ]
      },
      {
        "title": "Response Time",
        "type": "graph",
        "targets": [
          {
            "expr": "histogram_quantile(0.95, rate(http_request_duration_seconds_bucket[5m]))",
            "legendFormat": "95th percentile"
          }
        ]
      }
    ]
  }
}
```

### Logging Best Practices

**1. Structured Logging**
```javascript
// Application logging example
const logger = require('winston');

logger.info('User login attempt', {
  userId: user.id,
  email: user.email,
  ip: req.ip,
  userAgent: req.get('User-Agent'),
  timestamp: new Date().toISOString()
});

logger.error('Database connection failed', {
  error: err.message,
  stack: err.stack,
  database: 'users',
  timestamp: new Date().toISOString()
});
```

**2. Log Aggregation with ELK Stack**
```yaml
# filebeat.yml - Log shipping configuration
filebeat.inputs:
- type: container
  paths:
    - '/var/lib/docker/containers/*/*.log'

processors:
- add_kubernetes_metadata:
    host: ${NODE_NAME}
    matchers:
    - logs_path:
        logs_path: "/var/lib/docker/containers/"

output.elasticsearch:
  hosts: ["elasticsearch:9200"]
  index: "application-logs-%{+yyyy.MM.dd}"
```

## Common Pitfalls and How to Avoid Them

### Infrastructure as Code Mistakes

**1. Not Using Version Control**
```bash
# WRONG: Running terraform commands locally without version control
terraform apply

# RIGHT: All infrastructure code in git, applied through CI/CD
git add .
git commit -m "Add database configuration"
git push origin main
# CI/CD pipeline runs terraform apply
```

**2. Hardcoding Values**
```hcl
# WRONG: Hardcoded values
resource "aws_instance" "web" {
  ami           = "ami-12345678"  # This will break in other regions
  instance_type = "t3.micro"
  
  tags = {
    Name = "production-web-server"  # Not reusable
  }
}

# RIGHT: Using variables and data sources
data "aws_ami" "ubuntu" {
  most_recent = true
  owners      = ["099720109477"] # Canonical

  filter {
    name   = "name"
    values = ["ubuntu/images/hvm-ssd/ubuntu-focal-20.04-amd64-server-*"]
  }
}

resource "aws_instance" "web" {
  ami           = data.aws_ami.ubuntu.id
  instance_type = var.instance_type
  
  tags = {
    Name        = "${var.app_name}-${var.environment}-web"
    Environment = var.environment
  }
}
```

**3. Ignoring State Management**
```hcl
# WRONG: Local state (default)
# terraform.tfstate stored locally, can't collaborate

# RIGHT: Remote state
terraform {
  backend "s3" {
    bucket = "my-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-west-2"
    
    # Enable state locking
    dynamodb_table = "terraform-state-lock"
    encrypt        = true
  }
}
```

### Container Mistakes

**1. Running as Root**
```dockerfile
# WRONG: Running as root (security risk)
FROM node:18
COPY . /app
WORKDIR /app
RUN npm install
CMD ["npm", "start"]

# RIGHT: Create and use non-root user
FROM node:18
RUN groupadd -r appuser && useradd -r -g appuser appuser
COPY . /app
WORKDIR /app
RUN npm install && chown -R appuser:appuser /app
USER appuser
CMD ["npm", "start"]
```

**2. Large Images**
```dockerfile
# WRONG: Large base image with unnecessary tools
FROM ubuntu:latest
RUN apt-get update && apt-get install -y \
    nodejs npm python3 gcc g++ make \
    vim curl wget git
COPY . /app
WORKDIR /app
RUN npm install
CMD ["npm", "start"]

# RIGHT: Minimal base image, multi-stage build
FROM node:18-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

FROM node:18-alpine
RUN addgroup -g 1001 -S nodejs && adduser -S nodejs -u 1001
WORKDIR /app
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules
COPY --chown=nodejs:nodejs . .
USER nodejs
CMD ["npm", "start"]
```

### Kubernetes Mistakes

**1. No Resource Limits**
```yaml
# WRONG: No resource limits (can consume all cluster resources)
spec:
  containers:
  - name: web
    image: my-app:latest

# RIGHT: Set resource requests and limits
spec:
  containers:
  - name: web
    image: my-app:latest
    resources:
      requests:
        memory: "128Mi"
        cpu: "100m"
      limits:
        memory: "256Mi"
        cpu: "200m"
```

**2. Storing Secrets in Code**
```yaml
# WRONG: Secrets in plain text
env:
- name: DATABASE_PASSWORD
  value: "super-secret-password"

# RIGHT: Use Kubernetes secrets
env:
- name: DATABASE_PASSWORD
  valueFrom:
    secretKeyRef:
      name: db-secret
      key: password
```

## Next Steps

1. **Start with Docker**: Containerize a simple application you've built
2. **Learn Terraform Basics**: Provision simple cloud resources (VM, storage, network)
3. **Automate Deployment**: Create a simple CI/CD pipeline that deploys your containerized app
4. **Add Monitoring**: Implement basic health checks and logging
5. **Consider Kubernetes**: Only after you're comfortable with containers and have multiple services to orchestrate

Remember: Infrastructure as Code is about solving real problems, not learning tools for their own sake. Start with the problems you're facing and choose tools that solve them effectively.