---
name: devops-automation
description: Expert-level DevOps and infrastructure automation - CI/CD pipelines, IaC, containerization, orchestration, and operational excellence
keywords: [devops, ci-cd, infrastructure-as-code, terraform, kubernetes, docker, github-actions, jenkins, aws, azure, gcp, monitoring, observability, sre]
use_cases:
  - CI/CD pipeline design and implementation
  - Infrastructure as Code (Terraform, Pulumi, CloudFormation)
  - Container orchestration (Kubernetes, ECS, Docker Swarm)
  - Cloud architecture and multi-cloud strategies
  - GitOps and deployment automation
  - Monitoring, alerting, and observability systems
  - SRE practices and reliability engineering
  - Cost optimization and FinOps
license: MIT
---

# DevOps Automation Skill

> **Expertise Level**: Staff/Principal DevOps Engineer - Equivalent to 15+ years infrastructure and automation experience

## Purpose

Transform infrastructure and deployment challenges into automated, scalable, reliable systems. This skill provides expert-level guidance for CI/CD, Infrastructure as Code, container orchestration, and operational excellence—the core pillars of modern DevOps practice.

## Related Skills

- **security-audit** - Integrate security into DevOps (DevSecOps)
- **performance-optimization** - Infrastructure performance tuning
- **cost-optimization** - Cloud FinOps and resource efficiency
- **incident-response** - Production incident management
- **api-design** - Service architecture for deployments

---

## Core Process

### Phase 1: Infrastructure Assessment

```markdown
## Current State Analysis

### Infrastructure Inventory
| Component | Technology | State Management | Automation Level |
|-----------|------------|------------------|------------------|
| Compute | [EC2/GKE/AKS/VMs] | [Manual/IaC] | [0-100%] |
| Networking | [VPC/Subnets/LB] | [Manual/IaC] | [0-100%] |
| Storage | [S3/EBS/GCS] | [Manual/IaC] | [0-100%] |
| Database | [RDS/Cloud SQL/Atlas] | [Manual/IaC] | [0-100%] |
| Security | [IAM/Secrets/Certs] | [Manual/IaC] | [0-100%] |

### Deployment Analysis
| Application | Deploy Frequency | Deploy Duration | Rollback Time | Failure Rate |
|-------------|------------------|-----------------|---------------|--------------|
| [Service A] | [X/day|week|month] | [X minutes] | [X minutes] | [X%] |

### Pain Points
1. **Manual processes**: [List manual deployment steps]
2. **Environment drift**: [Configuration inconsistencies]
3. **Visibility gaps**: [Monitoring/logging deficiencies]
4. **Recovery challenges**: [Disaster recovery concerns]
```

### Phase 2: CI/CD Pipeline Design

```yaml
# GitHub Actions Expert Template
name: Production CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

env:
  REGISTRY: ghcr.io
  IMAGE_NAME: ${{ github.repository }}

jobs:
  # Stage 1: Quality Gates
  lint-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Set up language runtime
        uses: actions/setup-node@v4  # or python, go, etc.
        with:
          node-version: '20'
          cache: 'npm'
      
      - name: Install dependencies
        run: npm ci
      
      - name: Lint
        run: npm run lint
      
      - name: Unit Tests
        run: npm run test:unit -- --coverage
      
      - name: Upload coverage
        uses: codecov/codecov-action@v4

  # Stage 2: Security Scanning
  security:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      - name: Dependency Audit
        run: npm audit --audit-level=high
      
      - name: SAST Scan
        uses: github/codeql-action/analyze@v3
      
      - name: Secret Detection
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./
          base: ${{ github.event.repository.default_branch }}

  # Stage 3: Build & Push
  build:
    needs: [lint-and-test, security]
    runs-on: ubuntu-latest
    outputs:
      image-tag: ${{ steps.meta.outputs.tags }}
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v3
      
      - name: Login to Registry
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
            type=sha,prefix=
            type=ref,event=branch
            type=semver,pattern={{version}}
      
      - name: Build and Push
        uses: docker/build-push-action@v5
        with:
          context: .
          push: true
          tags: ${{ steps.meta.outputs.tags }}
          cache-from: type=gha
          cache-to: type=gha,mode=max

  # Stage 4: Deploy
  deploy-staging:
    needs: build
    runs-on: ubuntu-latest
    environment: staging
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to Staging
        uses: azure/k8s-deploy@v4  # or aws-actions/amazon-ecs-deploy
        with:
          manifests: k8s/staging/
          images: ${{ needs.build.outputs.image-tag }}

  # Stage 5: Integration Tests
  integration-tests:
    needs: deploy-staging
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run E2E Tests
        run: npm run test:e2e
        env:
          TEST_URL: ${{ vars.STAGING_URL }}

  # Stage 6: Production Deploy
  deploy-production:
    needs: integration-tests
    runs-on: ubuntu-latest
    environment: production
    steps:
      - uses: actions/checkout@v4
      
      - name: Deploy to Production
        uses: azure/k8s-deploy@v4
        with:
          manifests: k8s/production/
          images: ${{ needs.build.outputs.image-tag }}
          strategy: canary
          percentage: 20
      
      - name: Smoke Tests
        run: ./scripts/smoke-test.sh ${{ vars.PROD_URL }}
      
      - name: Promote Canary
        if: success()
        run: kubectl argo rollouts promote ${{ env.APP_NAME }}
```

### Phase 3: Infrastructure as Code

```hcl
# Terraform Expert Module Structure
# modules/eks-cluster/main.tf

terraform {
  required_version = ">= 1.5"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
    kubernetes = {
      source  = "hashicorp/kubernetes"
      version = "~> 2.23"
    }
  }
}

# -----------------------------------------------------------------------------
# EKS Cluster
# -----------------------------------------------------------------------------
module "eks" {
  source  = "terraform-aws-modules/eks/aws"
  version = "~> 19.0"

  cluster_name    = var.cluster_name
  cluster_version = var.kubernetes_version

  vpc_id     = var.vpc_id
  subnet_ids = var.private_subnet_ids

  # Control plane logging
  cluster_enabled_log_types = ["api", "audit", "authenticator", "controllerManager", "scheduler"]

  # Node groups with spot instances for cost optimization
  eks_managed_node_groups = {
    # System workloads - on-demand for stability
    system = {
      name           = "system"
      instance_types = ["m6i.large", "m5.large"]
      capacity_type  = "ON_DEMAND"
      
      min_size     = 2
      max_size     = 4
      desired_size = 2

      labels = {
        role = "system"
      }
      
      taints = []
    }

    # Application workloads - spot for cost savings
    application = {
      name           = "application"
      instance_types = ["m6i.xlarge", "m5.xlarge", "m6a.xlarge"]
      capacity_type  = "SPOT"
      
      min_size     = 3
      max_size     = 20
      desired_size = 5

      labels = {
        role = "application"
      }
    }
  }

  # Cluster addons
  cluster_addons = {
    coredns = {
      most_recent = true
    }
    kube-proxy = {
      most_recent = true
    }
    vpc-cni = {
      most_recent              = true
      service_account_role_arn = module.vpc_cni_irsa.iam_role_arn
    }
    aws-ebs-csi-driver = {
      most_recent              = true
      service_account_role_arn = module.ebs_csi_irsa.iam_role_arn
    }
  }

  tags = var.tags
}

# -----------------------------------------------------------------------------
# IRSA for VPC CNI
# -----------------------------------------------------------------------------
module "vpc_cni_irsa" {
  source  = "terraform-aws-modules/iam/aws//modules/iam-role-for-service-accounts-eks"
  version = "~> 5.0"

  role_name             = "${var.cluster_name}-vpc-cni"
  attach_vpc_cni_policy = true
  vpc_cni_enable_ipv4   = true

  oidc_providers = {
    main = {
      provider_arn               = module.eks.oidc_provider_arn
      namespace_service_accounts = ["kube-system:aws-node"]
    }
  }

  tags = var.tags
}

# -----------------------------------------------------------------------------
# Outputs
# -----------------------------------------------------------------------------
output "cluster_endpoint" {
  description = "EKS cluster endpoint"
  value       = module.eks.cluster_endpoint
}

output "cluster_certificate_authority_data" {
  description = "Base64 encoded certificate data"
  value       = module.eks.cluster_certificate_authority_data
  sensitive   = true
}
```

### Phase 4: Kubernetes Deployment Patterns

```yaml
# Production-Ready Kubernetes Manifests

# Namespace with resource quotas
---
apiVersion: v1
kind: Namespace
metadata:
  name: production
  labels:
    istio-injection: enabled
---
apiVersion: v1
kind: ResourceQuota
metadata:
  name: production-quota
  namespace: production
spec:
  hard:
    requests.cpu: "100"
    requests.memory: "200Gi"
    limits.cpu: "200"
    limits.memory: "400Gi"
    pods: "500"

# Deployment with all best practices
---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: api-service
  namespace: production
  labels:
    app: api-service
    version: v1
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 0
  selector:
    matchLabels:
      app: api-service
  template:
    metadata:
      labels:
        app: api-service
        version: v1
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "8080"
        prometheus.io/path: "/metrics"
    spec:
      serviceAccountName: api-service
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 1000
      
      # Pod anti-affinity for HA
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
            - weight: 100
              podAffinityTerm:
                labelSelector:
                  matchLabels:
                    app: api-service
                topologyKey: topology.kubernetes.io/zone
      
      # Topology spread for even distribution
      topologySpreadConstraints:
        - maxSkew: 1
          topologyKey: topology.kubernetes.io/zone
          whenUnsatisfiable: ScheduleAnyway
          labelSelector:
            matchLabels:
              app: api-service
      
      containers:
        - name: api
          image: ghcr.io/org/api-service:v1.2.3
          imagePullPolicy: IfNotPresent
          
          ports:
            - name: http
              containerPort: 8080
              protocol: TCP
          
          # Resource management
          resources:
            requests:
              cpu: 250m
              memory: 512Mi
            limits:
              cpu: 1000m
              memory: 1Gi
          
          # Health checks
          livenessProbe:
            httpGet:
              path: /health/live
              port: http
            initialDelaySeconds: 15
            periodSeconds: 10
            timeoutSeconds: 5
            failureThreshold: 3
          
          readinessProbe:
            httpGet:
              path: /health/ready
              port: http
            initialDelaySeconds: 5
            periodSeconds: 5
            timeoutSeconds: 3
            failureThreshold: 3
          
          startupProbe:
            httpGet:
              path: /health/live
              port: http
            initialDelaySeconds: 10
            periodSeconds: 5
            failureThreshold: 30
          
          # Security context
          securityContext:
            allowPrivilegeEscalation: false
            readOnlyRootFilesystem: true
            capabilities:
              drop: ["ALL"]
          
          # Environment from ConfigMap and Secrets
          envFrom:
            - configMapRef:
                name: api-service-config
            - secretRef:
                name: api-service-secrets
          
          volumeMounts:
            - name: tmp
              mountPath: /tmp
            - name: cache
              mountPath: /app/cache
      
      volumes:
        - name: tmp
          emptyDir: {}
        - name: cache
          emptyDir:
            sizeLimit: 1Gi

# HPA for autoscaling
---
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: api-service
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: api-service
  minReplicas: 3
  maxReplicas: 50
  metrics:
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 70
    - type: Resource
      resource:
        name: memory
        target:
          type: Utilization
          averageUtilization: 80
  behavior:
    scaleDown:
      stabilizationWindowSeconds: 300
      policies:
        - type: Percent
          value: 10
          periodSeconds: 60
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
        - type: Percent
          value: 100
          periodSeconds: 15
        - type: Pods
          value: 4
          periodSeconds: 15
      selectPolicy: Max

# PodDisruptionBudget for availability
---
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: api-service
  namespace: production
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: api-service
```

### Phase 5: Observability Stack

```yaml
# Prometheus + Grafana + Loki Stack

# Prometheus Configuration
---
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  name: api-service
  namespace: monitoring
spec:
  selector:
    matchLabels:
      app: api-service
  namespaceSelector:
    matchNames:
      - production
  endpoints:
    - port: http
      interval: 15s
      path: /metrics

# Grafana Dashboard ConfigMap (example)
---
apiVersion: v1
kind: ConfigMap
metadata:
  name: api-service-dashboard
  namespace: monitoring
  labels:
    grafana_dashboard: "1"
data:
  api-service.json: |
    {
      "title": "API Service Dashboard",
      "panels": [
        {
          "title": "Request Rate",
          "type": "graph",
          "targets": [{
            "expr": "sum(rate(http_requests_total{app=\"api-service\"}[5m])) by (status_code)"
          }]
        },
        {
          "title": "Latency P99",
          "type": "graph",
          "targets": [{
            "expr": "histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket{app=\"api-service\"}[5m])) by (le))"
          }]
        },
        {
          "title": "Error Rate",
          "type": "stat",
          "targets": [{
            "expr": "sum(rate(http_requests_total{app=\"api-service\",status_code=~\"5..\"}[5m])) / sum(rate(http_requests_total{app=\"api-service\"}[5m])) * 100"
          }]
        }
      ]
    }

# Alert Rules
---
apiVersion: monitoring.coreos.com/v1
kind: PrometheusRule
metadata:
  name: api-service-alerts
  namespace: monitoring
spec:
  groups:
    - name: api-service
      rules:
        - alert: HighErrorRate
          expr: |
            sum(rate(http_requests_total{app="api-service",status_code=~"5.."}[5m])) 
            / sum(rate(http_requests_total{app="api-service"}[5m])) > 0.05
          for: 5m
          labels:
            severity: critical
          annotations:
            summary: "High error rate on API service"
            description: "Error rate is {{ $value | humanizePercentage }}"
        
        - alert: HighLatency
          expr: |
            histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket{app="api-service"}[5m])) by (le)) > 1
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "High latency on API service"
            description: "P99 latency is {{ $value | humanizeDuration }}"
        
        - alert: PodNotReady
          expr: |
            kube_pod_status_ready{namespace="production",pod=~"api-service.*",condition="true"} == 0
          for: 5m
          labels:
            severity: warning
          annotations:
            summary: "Pod not ready"
            description: "Pod {{ $labels.pod }} has been not ready for 5 minutes"
```

---

## Expert Decision Frameworks

### CI/CD Strategy Selection

| Factor | GitHub Actions | GitLab CI | Jenkins | ArgoCD |
|--------|---------------|-----------|---------|--------|
| **Simplicity** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐ |
| **Scalability** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **GitOps Native** | ⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Self-hosted** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Enterprise features** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ |

### IaC Tool Selection

| Factor | Terraform | Pulumi | CloudFormation | CDK |
|--------|-----------|--------|----------------|-----|
| **Multi-cloud** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐ | ⭐⭐ |
| **Learning curve** | ⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |
| **State management** | ⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐⭐ |
| **Testing** | ⭐⭐⭐ | ⭐⭐⭐⭐⭐ | ⭐⭐ | ⭐⭐⭐⭐ |
| **Ecosystem** | ⭐⭐⭐⭐⭐ | ⭐⭐⭐⭐ | ⭐⭐⭐ | ⭐⭐⭐⭐ |

### Container Orchestration Selection

| Factor | Kubernetes | ECS | Docker Swarm | Nomad |
|--------|------------|-----|--------------|-------|
| **Complexity** | High | Medium | Low | Medium |
| **Scalability** | Excellent | Excellent | Good | Excellent |
| **Ecosystem** | Vast | AWS-focused | Limited | Growing |
| **Multi-cloud** | Yes | No | Yes | Yes |
| **Best for** | Large-scale | AWS-native | Simple deployments | Mixed workloads |

---

## Anti-Patterns to Avoid

| Anti-Pattern | Problem | Better Approach |
|--------------|---------|-----------------|
| **Snowflake servers** | Configuration drift, unreproducible | Immutable infrastructure with IaC |
| **Manual deployments** | Human error, slow, unauditable | Fully automated CI/CD pipelines |
| **No rollback plan** | Extended outages | Blue-green or canary with instant rollback |
| **Secrets in code** | Security vulnerabilities | External secret management (Vault, AWS Secrets) |
| **No resource limits** | Noisy neighbors, OOM kills | Resource requests and limits on all containers |
| **Single replica** | No fault tolerance | Minimum 3 replicas across AZs |
| **No health checks** | Traffic to unhealthy pods | Liveness, readiness, and startup probes |
| **Monolithic pipelines** | Slow feedback, hard to maintain | Modular, reusable pipeline components |

---

## SRE Golden Signals

```markdown
## Four Golden Signals Checklist

### 1. Latency
- [ ] P50, P95, P99 latency tracked
- [ ] Latency SLOs defined
- [ ] Alerts on latency degradation
- [ ] Latency broken down by endpoint

### 2. Traffic
- [ ] Request rate monitored
- [ ] Traffic patterns understood
- [ ] Capacity planning based on traffic
- [ ] Geographic distribution tracked

### 3. Errors
- [ ] Error rate calculated
- [ ] Error types categorized
- [ ] Error budget defined
- [ ] Alerts before budget exhaustion

### 4. Saturation
- [ ] CPU utilization tracked
- [ ] Memory utilization tracked
- [ ] Disk I/O monitored
- [ ] Network bandwidth measured
```

---

## Deployment Strategy Comparison

| Strategy | Downtime | Risk | Complexity | Rollback Speed | Use Case |
|----------|----------|------|------------|----------------|----------|
| **Recreate** | Yes | High | Low | Slow | Dev environments |
| **Rolling** | No | Medium | Low | Medium | Standard deployments |
| **Blue-Green** | No | Low | Medium | Instant | Critical services |
| **Canary** | No | Very Low | High | Fast | High-traffic services |
| **A/B Testing** | No | Low | High | Fast | Feature experiments |
| **Shadow** | No | Very Low | Very High | N/A | Critical migrations |

---

## Quick Reference Commands

```bash
# Terraform
terraform init -backend-config=env/prod.hcl
terraform plan -var-file=prod.tfvars -out=plan.out
terraform apply plan.out
terraform state list | grep aws_instance

# Kubernetes
kubectl get pods -A -o wide
kubectl top pods --containers
kubectl rollout status deployment/api-service
kubectl rollout undo deployment/api-service
kubectl exec -it pod/api-service-xxx -- /bin/sh

# Docker
docker build -t app:$(git rev-parse --short HEAD) .
docker system prune -af --volumes
docker stats --format "{{.Name}}: {{.CPUPerc}} / {{.MemUsage}}"

# Helm
helm upgrade --install app ./chart -f values-prod.yaml --atomic --timeout 10m
helm rollback app 1
helm template app ./chart | kubectl apply --dry-run=client -f -
```

---

## Version History

| Version | Date | Changes |
|---------|------|---------|
| 1.0 | 2025-01-13 | Initial expert skill creation |
