---
name: dev-devops
description: Design CI/CD pipelines, deployment strategies, and infrastructure configurations. Use when planning DevOps workflows, containerization, or cloud deployments.
---

# DevOps & Deployment Planning

## Purpose

Use this Skill to design DevOps infrastructure:
- CI/CD pipeline configuration
- Containerization with Docker
- Deployment strategies
- Infrastructure as Code
- Monitoring and observability

## Inputs

The main agent should provide:
- Application architecture
- Target deployment platform (Vercel, AWS, GCP, etc.)
- Scale requirements
- Team DevOps expertise level

## Process

### 1. CI/CD Pipeline Design

Design automated pipelines:

```yaml
# .github/workflows/ci.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  NODE_VERSION: '20'
  
jobs:
  # Stage 1: Lint and Type Check
  lint:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - run: npm ci
      - run: npm run lint
      - run: npm run type-check

  # Stage 2: Unit Tests
  test:
    runs-on: ubuntu-latest
    needs: lint
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ env.NODE_VERSION }}
          cache: 'npm'
      
      - run: npm ci
      - run: npm run test:unit -- --coverage
      
      - uses: codecov/codecov-action@v3

  # Stage 3: Integration Tests
  integration:
    runs-on: ubuntu-latest
    needs: lint
    services:
      postgres:
        image: postgres:15
        env:
          POSTGRES_PASSWORD: test
          POSTGRES_DB: test
        ports:
          - 5432:5432
        options: >-
          --health-cmd pg_isready
          --health-interval 10s
          --health-timeout 5s
          --health-retries 5
      redis:
        image: redis:7
        ports:
          - 6379:6379
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      
      - run: npm ci
      - run: npm run test:integration
        env:
          DATABASE_URL: postgresql://postgres:test@localhost:5432/test
          REDIS_URL: redis://localhost:6379

  # Stage 4: Build
  build:
    runs-on: ubuntu-latest
    needs: [test, integration]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
      
      - run: npm ci
      - run: npm run build
      
      - uses: actions/upload-artifact@v3
        with:
          name: build
          path: dist/

  # Stage 5: Deploy (on main only)
  deploy:
    runs-on: ubuntu-latest
    needs: build
    if: github.ref == 'refs/heads/main'
    environment: production
    
    steps:
      - uses: actions/checkout@v4
      - uses: actions/download-artifact@v3
        with:
          name: build
          path: dist/
      
      # Deploy to Vercel/AWS/GCP
      - name: Deploy to Production
        run: |
          # Deployment commands
```

### 2. Docker Configuration

Design container setup:

```dockerfile
# Dockerfile (multi-stage build)

# Stage 1: Dependencies
FROM node:20-alpine AS deps
WORKDIR /app
COPY package*.json ./
RUN npm ci --only=production

# Stage 2: Builder
FROM node:20-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage 3: Runner
FROM node:20-alpine AS runner
WORKDIR /app

ENV NODE_ENV=production
ENV PORT=3000

# Security: Don't run as root
RUN addgroup --system --gid 1001 nodejs
RUN adduser --system --uid 1001 appuser

# Copy only necessary files
COPY --from=deps /app/node_modules ./node_modules
COPY --from=builder /app/dist ./dist
COPY --from=builder /app/package.json ./

USER appuser

EXPOSE 3000

HEALTHCHECK --interval=30s --timeout=3s \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

CMD ["node", "dist/main.js"]
```

```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - '3000:3000'
    environment:
      - DATABASE_URL=postgresql://postgres:password@db:5432/myapp
      - REDIS_URL=redis://redis:6379
    depends_on:
      db:
        condition: service_healthy
      redis:
        condition: service_started
    healthcheck:
      test: ['CMD', 'wget', '--spider', 'http://localhost:3000/health']
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=password
      - POSTGRES_DB=myapp
    healthcheck:
      test: ['CMD-SHELL', 'pg_isready -U postgres']
      interval: 10s
      timeout: 5s
      retries: 5

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### 3. Environment Configuration

Plan environment management:

```bash
# Environment hierarchy
.env.example          # Template (committed)
.env                  # Local development (gitignored)
.env.test             # Test environment
.env.production       # Production (never committed)

# Secrets management
# Use platform-specific solutions:
# - GitHub Actions: Secrets
# - AWS: Secrets Manager / Parameter Store
# - GCP: Secret Manager
# - Vercel: Environment Variables
```

```typescript
// config/configuration.ts
import { z } from 'zod';

const envSchema = z.object({
  NODE_ENV: z.enum(['development', 'test', 'production']).default('development'),
  PORT: z.string().transform(Number).default('3000'),
  DATABASE_URL: z.string().url(),
  REDIS_URL: z.string().url().optional(),
  JWT_SECRET: z.string().min(32),
  JWT_EXPIRY: z.string().default('7d'),
});

export type Env = z.infer<typeof envSchema>;

export function validateEnv(): Env {
  const result = envSchema.safeParse(process.env);
  
  if (!result.success) {
    console.error('❌ Invalid environment variables:');
    console.error(result.error.format());
    process.exit(1);
  }
  
  return result.data;
}
```

### 4. Deployment Strategies

Plan deployment approach:

```yaml
# Blue-Green Deployment
# Two identical environments, switch traffic

# Canary Deployment
# Gradual rollout to subset of users

# Rolling Deployment
# Update instances one at a time

# Kubernetes deployment example
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1
      maxSurge: 1
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      labels:
        app: myapp
    spec:
      containers:
        - name: myapp
          image: myapp:latest
          ports:
            - containerPort: 3000
          resources:
            requests:
              cpu: '100m'
              memory: '128Mi'
            limits:
              cpu: '500m'
              memory: '512Mi'
          livenessProbe:
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 30
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /health/ready
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 5
```

### 5. Monitoring & Observability

Design monitoring stack:

```typescript
// Health check endpoints
@Controller('health')
export class HealthController {
  constructor(
    private health: HealthCheckService,
    private db: TypeOrmHealthIndicator,
    private redis: RedisHealthIndicator,
  ) {}

  @Get()
  @HealthCheck()
  check() {
    return this.health.check([
      () => this.db.pingCheck('database'),
      () => this.redis.pingCheck('redis'),
    ]);
  }

  @Get('ready')
  ready() {
    return { status: 'ready' };
  }
}

// Logging configuration
import { WinstonModule } from 'nest-winston';
import * as winston from 'winston';

const logger = WinstonModule.createLogger({
  transports: [
    new winston.transports.Console({
      format: winston.format.combine(
        winston.format.timestamp(),
        winston.format.json(),
      ),
    }),
  ],
});

// Metrics (Prometheus)
import { PrometheusModule } from '@willsoto/nestjs-prometheus';

@Module({
  imports: [
    PrometheusModule.register({
      defaultMetrics: {
        enabled: true,
      },
    }),
  ],
})
export class AppModule {}

// Custom metrics
import { Counter, Histogram } from 'prom-client';

const httpRequestDuration = new Histogram({
  name: 'http_request_duration_seconds',
  help: 'Duration of HTTP requests in seconds',
  labelNames: ['method', 'route', 'status'],
});

const httpRequestTotal = new Counter({
  name: 'http_requests_total',
  help: 'Total number of HTTP requests',
  labelNames: ['method', 'route', 'status'],
});
```

### 6. Infrastructure as Code

Plan IaC approach:

```hcl
# Terraform example for AWS

# main.tf
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  
  backend "s3" {
    bucket = "myapp-terraform-state"
    key    = "prod/terraform.tfstate"
    region = "us-east-1"
  }
}

# VPC
module "vpc" {
  source = "terraform-aws-modules/vpc/aws"
  
  name = "myapp-vpc"
  cidr = "10.0.0.0/16"
  
  azs             = ["us-east-1a", "us-east-1b"]
  private_subnets = ["10.0.1.0/24", "10.0.2.0/24"]
  public_subnets  = ["10.0.101.0/24", "10.0.102.0/24"]
  
  enable_nat_gateway = true
}

# RDS PostgreSQL
module "rds" {
  source = "terraform-aws-modules/rds/aws"
  
  identifier = "myapp-db"
  
  engine            = "postgres"
  engine_version    = "15"
  instance_class    = "db.t3.micro"
  allocated_storage = 20
  
  db_name  = "myapp"
  username = "postgres"
  port     = 5432
  
  vpc_security_group_ids = [module.security_group.security_group_id]
  subnet_ids             = module.vpc.private_subnets
  
  backup_retention_period = 7
  deletion_protection     = true
}

# ECS Fargate
resource "aws_ecs_service" "myapp" {
  name            = "myapp"
  cluster         = aws_ecs_cluster.main.id
  task_definition = aws_ecs_task_definition.myapp.arn
  desired_count   = 2
  launch_type     = "FARGATE"
  
  network_configuration {
    subnets         = module.vpc.private_subnets
    security_groups = [aws_security_group.ecs.id]
  }
  
  load_balancer {
    target_group_arn = aws_lb_target_group.myapp.arn
    container_name   = "myapp"
    container_port   = 3000
  }
}
```

### 7. Security Hardening

Plan security measures:

```yaml
# Security checklist
- [ ] HTTPS everywhere (TLS 1.3)
- [ ] Security headers (Helmet.js)
- [ ] Rate limiting
- [ ] CORS configuration
- [ ] WAF rules
- [ ] Secrets rotation
- [ ] Container scanning
- [ ] Dependency scanning
- [ ] Penetration testing

# Security headers example
app.use(helmet({
  contentSecurityPolicy: {
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", "'unsafe-inline'"],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", "data:", "https:"],
    },
  },
  hsts: {
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true,
  },
}));

# Rate limiting
app.use(rateLimit({
  windowMs: 15 * 60 * 1000, // 15 minutes
  max: 100, // Limit each IP
  standardHeaders: true,
  legacyHeaders: false,
}));
```

## Output Format

Produce documentation with:
- CI/CD pipeline diagram
- Dockerfile and docker-compose
- Environment configuration
- Deployment strategy
- Monitoring setup
- IaC templates
- Security checklist

## Best Practices

- **Automate everything**: No manual deployments
- **Infrastructure as Code**: Version control infra
- **Immutable deployments**: Don't modify running containers
- **Health checks**: Always implement liveness/readiness
- **Secrets management**: Never commit secrets
- **Observability**: Logs, metrics, traces
