# CI/CD Pipeline Fundamentals

## Overview

Continuous Integration and Continuous Deployment (CI/CD) automates the process of testing, building, and deploying your applications. Instead of manual deployments that are error-prone and time-consuming, CI/CD pipelines ensure consistent, reliable, and fast delivery of software changes.

**Why CI/CD Matters:**
- Catch bugs early through automated testing
- Reduce deployment risk through consistent processes
- Enable faster feature delivery
- Improve team collaboration and code quality
- Provide rollback capabilities when issues occur

**What Problems CI/CD Solves:**
- Manual deployment errors and inconsistencies
- Long feedback loops between code changes and production
- Fear of deploying due to complexity
- Inability to quickly rollback problematic changes
- Lack of deployment visibility and audit trails

## CI/CD Fundamentals

### Continuous Integration (CI)

**The Problem:** Developers work on separate features, and integrating their changes often breaks the application.

**The Solution:** Automatically test every code change when it's pushed to version control.

**Core CI Principles:**
1. **Frequent Integration**: Merge code changes multiple times per day
2. **Automated Testing**: Run tests automatically on every change
3. **Fast Feedback**: Developers know within minutes if their changes break anything
4. **Shared Repository**: All code changes go through the same integration process

### Continuous Deployment (CD)

**The Problem:** Even with good CI, manual deployments are slow, error-prone, and create bottlenecks.

**The Solution:** Automatically deploy code changes that pass all tests.

**Core CD Principles:**
1. **Automated Deployment**: No manual steps in the deployment process
2. **Environment Consistency**: Same deployment process for all environments
3. **Rollback Capability**: Quick and reliable way to undo deployments
4. **Deployment Monitoring**: Visibility into deployment status and health

## GitHub Actions Fundamentals

### Why GitHub Actions?

**Advantages:**
- Integrated with GitHub repositories
- Free tier for public repositories and generous limits for private
- Extensive marketplace of pre-built actions
- Simple YAML configuration
- Good documentation and community support

**When to Choose GitHub Actions:**
- Your code is already on GitHub
- You want simple, integrated CI/CD
- You're building web applications, APIs, or mobile apps
- You need basic to moderate complexity workflows

### Basic GitHub Actions Workflow

**1. Simple Node.js CI Pipeline**
```yaml
# .github/workflows/ci.yml
name: CI Pipeline

on:
  push:
    branches: [ main, develop ]
  pull_request:
    branches: [ main ]

jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        node-version: [16.x, 18.x, 20.x]
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run linting
      run: npm run lint
    
    - name: Run tests
      run: npm test
    
    - name: Run security audit
      run: npm audit --audit-level high
```

**2. Build and Deploy Pipeline**
```yaml
# .github/workflows/deploy.yml
name: Build and Deploy

on:
  push:
    branches: [ main ]

env:
  NODE_VERSION: '18.x'
  
jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Node.js
      uses: actions/setup-node@v4
      with:
        node-version: ${{ env.NODE_VERSION }}
        cache: 'npm'
    
    - name: Install dependencies
      run: npm ci
    
    - name: Run tests
      run: npm test
    
    - name: Build application
      run: npm run build
    
    - name: Upload build artifacts
      uses: actions/upload-artifact@v4
      with:
        name: build-files
        path: dist/
        retention-days: 30

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
    - name: Download build artifacts
      uses: actions/download-artifact@v4
      with:
        name: build-files
        path: dist/
    
    - name: Deploy to staging
      run: |
        echo "Deploying to staging environment"
        # Add your deployment commands here
    
    - name: Run smoke tests
      run: |
        echo "Running smoke tests against staging"
        # Add smoke test commands here
    
    - name: Deploy to production
      if: success()
      run: |
        echo "Deploying to production environment"
        # Add production deployment commands here
```

### Docker Integration with GitHub Actions

**Building and Pushing Docker Images:**
```yaml
# .github/workflows/docker.yml
name: Docker Build and Push

on:
  push:
    branches: [ main ]
    tags: [ 'v*' ]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  build-and-push:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      packages: write
    
    steps:
    - name: Checkout code
      uses: actions/checkout@v4
    
    - name: Setup Docker Buildx
      uses: docker/setup-buildx-action@v3
    
    - name: Login to Container Registry
      uses: docker/login-action@v3
      with:
        registry: ${{ env.REGISTRY }}
        username: ${{ github.actor }}
        password: ${{ secrets.GITHUB_TOKEN }}
    
    - name: Extract metadata
      id: meta
      uses: docker/metadata-action@v5
      with:
        images: ${{ env.REGISTRY }}/${{ env.IMAGE_NAME }}
        tags: |
          type=ref,event=branch
          type=ref,event=pr
          type=semver,pattern={{version}}
          type=semver,pattern={{major}}.{{minor}}
    
    - name: Build and push Docker image
      uses: docker/build-push-action@v5
      with:
        context: .
        platforms: linux/amd64,linux/arm64
        push: true
        tags: ${{ steps.meta.outputs.tags }}
        labels: ${{ steps.meta.outputs.labels }}
        cache-from: type=gha
        cache-to: type=gha,mode=max
```

### Advanced GitHub Actions Patterns

**1. Environment-Specific Deployments**
```yaml
# .github/workflows/environment-deploy.yml
name: Environment Deployments

on:
  push:
    branches: [ main, develop ]

jobs:
  deploy-staging:
    if: github.ref == 'refs/heads/develop'
    runs-on: ubuntu-latest
    environment: staging
    
    steps:
    - name: Deploy to Staging
      run: |
        echo "Deploying ${{ github.sha }} to staging"
        # Staging deployment commands
    
    - name: Run integration tests
      run: |
        echo "Running integration tests"
        # Integration test commands

  deploy-production:
    if: github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    environment: production
    needs: [build, test]
    
    steps:
    - name: Deploy to Production
      run: |
        echo "Deploying ${{ github.sha }} to production"
        # Production deployment commands
    
    - name: Notify team
      uses: 8398a7/action-slack@v3
      with:
        status: ${{ job.status }}
        channel: '#deployments'
        webhook_url: ${{ secrets.SLACK_WEBHOOK }}
```

**2. Matrix Builds for Multiple Environments**
```yaml
# .github/workflows/matrix-build.yml
name: Multi-Environment Build

on: [push, pull_request]

jobs:
  build:
    runs-on: ${{ matrix.os }}
    
    strategy:
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [16.x, 18.x, 20.x]
        include:
          - os: ubuntu-latest
            node-version: 20.x
            deploy: true
    
    steps:
    - uses: actions/checkout@v4
    
    - name: Setup Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v4
      with:
        node-version: ${{ matrix.node-version }}
    
    - name: Install and test
      run: |
        npm ci
        npm test
    
    - name: Deploy
      if: matrix.deploy && github.ref == 'refs/heads/main'
      run: echo "Deploying from Ubuntu with Node 20.x"
```

## Jenkins Pipeline Fundamentals

### When to Choose Jenkins

**Use Jenkins When:**
- You need complex, custom build processes
- You're in an enterprise environment with specific security requirements
- You need extensive plugin ecosystem integration
- You want full control over your CI/CD infrastructure
- You have dedicated DevOps team to manage Jenkins

**Don't Use Jenkins If:**
- You want simple, managed CI/CD (use GitHub Actions, GitLab CI, etc.)
- You have a small team without dedicated DevOps resources
- You're building simple web applications or APIs
- You want to minimize infrastructure management overhead

### Jenkins Pipeline Syntax

**1. Declarative Pipeline (Recommended)**
```groovy
// Jenkinsfile
pipeline {
    agent any
    
    environment {
        NODE_VERSION = '18'
        DOCKER_REGISTRY = 'your-registry.com'
        IMAGE_NAME = 'my-web-app'
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Setup') {
            steps {
                sh 'node --version'
                sh 'npm --version'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }
        
        stage('Lint and Test') {
            parallel {
                stage('Lint') {
                    steps {
                        sh 'npm run lint'
                    }
                }
                stage('Unit Tests') {
                    steps {
                        sh 'npm test'
                    }
                    post {
                        always {
                            publishTestResults testResultsPattern: 'test-results.xml'
                        }
                    }
                }
                stage('Security Audit') {
                    steps {
                        sh 'npm audit --audit-level high'
                    }
                }
            }
        }
        
        stage('Build') {
            steps {
                sh 'npm run build'
                archiveArtifacts artifacts: 'dist/**', fingerprint: true
            }
        }
        
        stage('Docker Build') {
            when {
                branch 'main'
            }
            steps {
                script {
                    def image = docker.build("${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER}")
                    docker.withRegistry("https://${DOCKER_REGISTRY}", 'docker-registry-credentials') {
                        image.push()
                        image.push('latest')
                    }
                }
            }
        }
        
        stage('Deploy to Staging') {
            when {
                branch 'develop'
            }
            steps {
                sh '''
                    echo "Deploying to staging environment"
                    kubectl set image deployment/web-app web=${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} -n staging
                    kubectl rollout status deployment/web-app -n staging
                '''
            }
        }
        
        stage('Deploy to Production') {
            when {
                branch 'main'
            }
            steps {
                input message: 'Deploy to production?', ok: 'Deploy'
                sh '''
                    echo "Deploying to production environment"
                    kubectl set image deployment/web-app web=${DOCKER_REGISTRY}/${IMAGE_NAME}:${BUILD_NUMBER} -n production
                    kubectl rollout status deployment/web-app -n production
                '''
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            slackSend channel: '#deployments',
                     color: 'good',
                     message: "✅ Pipeline succeeded: ${env.JOB_NAME} - ${env.BUILD_NUMBER}"
        }
        failure {
            slackSend channel: '#deployments',
                     color: 'danger',
                     message: "❌ Pipeline failed: ${env.JOB_NAME} - ${env.BUILD_NUMBER}"
        }
    }
}
```

**2. Multi-Branch Pipeline Configuration**
```groovy
// Jenkinsfile for multi-branch pipeline
pipeline {
    agent {
        kubernetes {
            yaml """
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                  - name: node
                    image: node:18-alpine
                    command:
                    - cat
                    tty: true
                  - name: docker
                    image: docker:latest
                    command:
                    - cat
                    tty: true
                    volumeMounts:
                    - mountPath: /var/run/docker.sock
                      name: docker-sock
                  volumes:
                  - name: docker-sock
                    hostPath:
                      path: /var/run/docker.sock
            """
        }
    }
    
    environment {
        BRANCH_NAME = "${env.BRANCH_NAME}"
        BUILD_NUMBER = "${env.BUILD_NUMBER}"
    }
    
    stages {
        stage('Build and Test') {
            steps {
                container('node') {
                    sh '''
                        npm ci
                        npm run lint
                        npm test
                        npm run build
                    '''
                }
            }
        }
        
        stage('Docker Build and Push') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                container('docker') {
                    sh '''
                        docker build -t my-app:${BRANCH_NAME}-${BUILD_NUMBER} .
                        docker tag my-app:${BRANCH_NAME}-${BUILD_NUMBER} my-registry/my-app:${BRANCH_NAME}-${BUILD_NUMBER}
                        docker push my-registry/my-app:${BRANCH_NAME}-${BUILD_NUMBER}
                    '''
                }
            }
        }
        
        stage('Deploy') {
            when {
                anyOf {
                    branch 'main'
                    branch 'develop'
                }
            }
            steps {
                script {
                    def environment = env.BRANCH_NAME == 'main' ? 'production' : 'staging'
                    sh "kubectl set image deployment/web-app web=my-registry/my-app:${BRANCH_NAME}-${BUILD_NUMBER} -n ${environment}"
                }
            }
        }
    }
}
```

## Automated Testing Integration

### Testing Pyramid in CI/CD

**1. Unit Tests (Fast, Many)**
```yaml
# GitHub Actions example
- name: Run Unit Tests
  run: |
    npm test -- --coverage --watchAll=false
    
- name: Upload Coverage Reports
  uses: codecov/codecov-action@v3
  with:
    file: ./coverage/lcov.info
    flags: unittests
```

**2. Integration Tests (Medium Speed, Some)**
```yaml
- name: Start Test Database
  run: |
    docker run -d --name test-db \
      -e POSTGRES_PASSWORD=test \
      -e POSTGRES_DB=testdb \
      -p 5432:5432 \
      postgres:15

- name: Run Integration Tests
  env:
    DATABASE_URL: postgresql://postgres:test@localhost:5432/testdb
  run: npm run test:integration

- name: Cleanup
  if: always()
  run: docker rm -f test-db
```

**3. End-to-End Tests (Slow, Few)**
```yaml
- name: Start Application
  run: |
    npm run build
    npm start &
    sleep 10  # Wait for app to start

- name: Run E2E Tests
  run: |
    npx playwright test
    
- name: Upload Test Results
  if: always()
  uses: actions/upload-artifact@v4
  with:
    name: playwright-report
    path: playwright-report/
```

### Test Automation Best Practices

**1. Fail Fast Strategy**
```yaml
jobs:
  quick-checks:
    runs-on: ubuntu-latest
    steps:
    - uses: actions/checkout@v4
    - name: Lint Check (Fast Fail)
      run: npm run lint
    - name: Type Check (Fast Fail)
      run: npm run type-check

  full-test-suite:
    needs: quick-checks  # Only run if quick checks pass
    runs-on: ubuntu-latest
    steps:
    - name: Run Full Test Suite
      run: npm test
```

**2. Parallel Test Execution**
```yaml
test:
  runs-on: ubuntu-latest
  strategy:
    matrix:
      test-group: [unit, integration, e2e]
  
  steps:
  - uses: actions/checkout@v4
  - name: Run ${{ matrix.test-group }} tests
    run: npm run test:${{ matrix.test-group }}
```

## Deployment Strategies

### Blue-Green Deployment

**The Concept:** Maintain two identical production environments. Deploy to the inactive one, test it, then switch traffic.

**GitHub Actions Implementation:**
```yaml
deploy-blue-green:
  runs-on: ubuntu-latest
  steps:
  - name: Determine Current Environment
    id: current
    run: |
      CURRENT=$(kubectl get service web-app -o jsonpath='{.spec.selector.version}')
      if [ "$CURRENT" = "blue" ]; then
        echo "target=green" >> $GITHUB_OUTPUT
      else
        echo "target=blue" >> $GITHUB_OUTPUT
      fi

  - name: Deploy to ${{ steps.current.outputs.target }}
    run: |
      kubectl set image deployment/web-app-${{ steps.current.outputs.target }} \
        web=my-app:${{ github.sha }}
      kubectl rollout status deployment/web-app-${{ steps.current.outputs.target }}

  - name: Run Health Checks
    run: |
      # Test the new deployment
      kubectl port-forward service/web-app-${{ steps.current.outputs.target }} 8080:80 &
      sleep 5
      curl -f http://localhost:8080/health

  - name: Switch Traffic
    run: |
      kubectl patch service web-app -p '{"spec":{"selector":{"version":"${{ steps.current.outputs.target }}"}}}'
```

### Canary Deployment

**The Concept:** Gradually roll out changes to a small percentage of users, monitor metrics, then proceed or rollback.

**Kubernetes Canary with Istio:**
```yaml
# canary-deployment.yaml
apiVersion: argoproj.io/v1alpha1
kind: Rollout
metadata:
  name: web-app-rollout
spec:
  replicas: 10
  strategy:
    canary:
      steps:
      - setWeight: 10    # 10% of traffic to new version
      - pause: {duration: 2m}
      - setWeight: 25    # 25% of traffic
      - pause: {duration: 5m}
      - setWeight: 50    # 50% of traffic
      - pause: {duration: 10m}
      - setWeight: 100   # Full rollout
      
      trafficRouting:
        istio:
          virtualService:
            name: web-app-vs
          destinationRule:
            name: web-app-dr
            
      analysis:
        templates:
        - templateName: success-rate
        args:
        - name: service-name
          value: web-app
```

### Rolling Deployment

**The Concept:** Gradually replace old instances with new ones, maintaining service availability.

**Kubernetes Rolling Update:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
spec:
  replicas: 6
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxUnavailable: 1      # At most 1 pod unavailable
      maxSurge: 2            # At most 2 extra pods during update
  template:
    spec:
      containers:
      - name: web
        image: my-app:latest
        readinessProbe:        # Ensure new pods are ready before routing traffic
          httpGet:
            path: /health
            port: 3000
          initialDelaySeconds: 10
          periodSeconds: 5
```

## Rollback Procedures

### Automated Rollback Triggers

**1. Health Check Based Rollback**
```yaml
# .github/workflows/deploy-with-rollback.yml
deploy-with-monitoring:
  runs-on: ubuntu-latest
  steps:
  - name: Deploy New Version
    id: deploy
    run: |
      kubectl set image deployment/web-app web=my-app:${{ github.sha }}
      kubectl rollout status deployment/web-app --timeout=300s

  - name: Wait for Stabilization
    run: sleep 60

  - name: Health Check
    id: health
    run: |
      # Check application health metrics
      HEALTH_SCORE=$(curl -s http://my-app.com/metrics | grep health_score | awk '{print $2}')
      if (( $(echo "$HEALTH_SCORE < 0.95" | bc -l) )); then
        echo "Health check failed: $HEALTH_SCORE"
        exit 1
      fi

  - name: Rollback on Failure
    if: failure()
    run: |
      echo "Rolling back due to health check failure"
      kubectl rollout undo deployment/web-app
      kubectl rollout status deployment/web-app
```

**2. Metric-Based Rollback**
```bash
#!/bin/bash
# rollback-monitor.sh

DEPLOYMENT_TIME=$(date +%s)
ROLLBACK_THRESHOLD=300  # 5 minutes

while true; do
  CURRENT_TIME=$(date +%s)
  ELAPSED=$((CURRENT_TIME - DEPLOYMENT_TIME))
  
  if [ $ELAPSED -gt $ROLLBACK_THRESHOLD ]; then
    echo "Monitoring period complete"
    break
  fi
  
  # Check error rate
  ERROR_RATE=$(curl -s "http://prometheus:9090/api/v1/query?query=rate(http_requests_total{status=~'5..'}[5m])" | jq -r '.data.result[0].value[1]')
  
  if (( $(echo "$ERROR_RATE > 0.05" | bc -l) )); then
    echo "Error rate too high: $ERROR_RATE. Rolling back..."
    kubectl rollout undo deployment/web-app
    exit 1
  fi
  
  sleep 30
done
```

### Manual Rollback Procedures

**1. Kubernetes Rollback Commands**
```bash
# View rollout history
kubectl rollout history deployment/web-app

# Rollback to previous version
kubectl rollout undo deployment/web-app

# Rollback to specific revision
kubectl rollout undo deployment/web-app --to-revision=3

# Check rollback status
kubectl rollout status deployment/web-app
```

**2. Database Migration Rollbacks**
```yaml
# In your CI/CD pipeline
- name: Backup Database
  run: |
    pg_dump $DATABASE_URL > backup-$(date +%Y%m%d-%H%M%S).sql
    aws s3 cp backup-*.sql s3://my-backups/

- name: Run Migrations
  run: npm run migrate

- name: Rollback Migrations on Failure
  if: failure()
  run: |
    echo "Rolling back database migrations"
    npm run migrate:rollback
```

## Common CI/CD Pitfalls and Solutions

### Pipeline Design Mistakes

**1. Overly Complex Pipelines**
```yaml
# WRONG: Everything in one massive pipeline
jobs:
  everything:
    steps:
    - name: Lint, test, build, deploy, notify, backup, monitor...
      run: |
        # 200 lines of bash commands

# RIGHT: Separate concerns
jobs:
  test:
    steps:
    - name: Run tests
      run: npm test
  
  build:
    needs: test
    steps:
    - name: Build application
      run: npm run build
  
  deploy:
    needs: build
    if: github.ref == 'refs/heads/main'
    steps:
    - name: Deploy to production
      run: ./deploy.sh
```

**2. No Environment Parity**
```yaml
# WRONG: Different deployment processes for different environments
deploy-staging:
  run: docker run my-app:latest  # Simple container

deploy-production:
  run: |
    # Complex Kubernetes deployment with different configuration
    kubectl apply -f complex-prod-config.yaml

# RIGHT: Same deployment process, different configuration
deploy:
  steps:
  - name: Deploy to ${{ matrix.environment }}
    run: |
      helm upgrade --install my-app ./chart \
        --values values-${{ matrix.environment }}.yaml
  strategy:
    matrix:
      environment: [staging, production]
```

**3. Ignoring Security**
```yaml
# WRONG: Secrets in plain text
env:
  DATABASE_PASSWORD: "super-secret-password"
  API_KEY: "sk-1234567890abcdef"

# RIGHT: Use secret management
env:
  DATABASE_PASSWORD: ${{ secrets.DATABASE_PASSWORD }}
  API_KEY: ${{ secrets.API_KEY }}

# Even better: Use OIDC for cloud authentication
- name: Configure AWS credentials
  uses: aws-actions/configure-aws-credentials@v4
  with:
    role-to-assume: arn:aws:iam::123456789012:role/GitHubActions
    aws-region: us-east-1
```

### Testing Integration Issues

**1. Flaky Tests**
```yaml
# WRONG: Ignoring test failures
- name: Run tests
  run: npm test || true  # Always passes

# RIGHT: Fix flaky tests, use retries sparingly
- name: Run tests with retry
  uses: nick-invision/retry@v2
  with:
    timeout_minutes: 10
    max_attempts: 3
    command: npm test
```

**2. No Test Environment Cleanup**
```yaml
# WRONG: Tests interfere with each other
- name: Run integration tests
  run: npm run test:integration

# RIGHT: Clean environment between tests
- name: Setup test environment
  run: |
    docker-compose -f docker-compose.test.yml up -d
    sleep 10

- name: Run integration tests
  run: npm run test:integration

- name: Cleanup test environment
  if: always()
  run: docker-compose -f docker-compose.test.yml down -v
```

### Deployment Issues

**1. No Rollback Strategy**
```yaml
# WRONG: Deploy and hope for the best
- name: Deploy
  run: kubectl apply -f deployment.yaml

# RIGHT: Deploy with rollback capability
- name: Deploy with rollback capability
  run: |
    kubectl apply -f deployment.yaml
    kubectl rollout status deployment/web-app --timeout=300s
    
- name: Rollback on failure
  if: failure()
  run: kubectl rollout undo deployment/web-app
```

**2. No Health Checks**
```yaml
# WRONG: Assume deployment worked
- name: Deploy
  run: ./deploy.sh

# RIGHT: Verify deployment health
- name: Deploy
  run: ./deploy.sh

- name: Health check
  run: |
    for i in {1..30}; do
      if curl -f http://my-app.com/health; then
        echo "Health check passed"
        exit 0
      fi
      sleep 10
    done
    echo "Health check failed"
    exit 1
```

## Next Steps

1. **Start Simple**: Begin with basic CI (automated testing on every commit)
2. **Add CD Gradually**: Start with automated deployment to staging, then production
3. **Implement Monitoring**: Add health checks and basic monitoring before complex deployments
4. **Choose Your Tools**: Pick one CI/CD platform and master it before exploring others
5. **Focus on Reliability**: Prioritize rollback capabilities and monitoring over complex deployment strategies

Remember: CI/CD is about reducing risk and increasing confidence in deployments, not about using the latest tools. Start with solving real problems you're experiencing with manual processes.