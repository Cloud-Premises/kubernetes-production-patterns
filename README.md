<div align="center">

# 🐳 Kubernetes Production Patterns

**Battle-Tested Kubernetes Configurations for Enterprise Workloads**

[![License: Apache 2.0](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](LICENSE)
[![Kubernetes](https://img.shields.io/badge/Kubernetes-1.28+-326CE5?logo=kubernetes)](https://kubernetes.io/)
[![Helm](https://img.shields.io/badge/Helm-3.0+-0F1689?logo=helm)](https://helm.sh/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

[Features](#-features) • [Installation](#-installation) • [Patterns](#-production-patterns) • [Documentation](#-documentation) • [Contributing](#-contributing)

</div>

---

## 📖 Table of Contents

- [Problem Statement](#-problem-statement)
- [Solution](#-solution)
- [Features](#-features)
- [Technology Stack](#-technology-stack)
- [Architecture](#-architecture)
- [Prerequisites](#-prerequisites)
- [Installation](#-installation)
- [Production Patterns](#-production-patterns)
- [Helm Charts](#-helm-charts)
- [Deployment Strategies](#-deployment-strategies)
- [Best Practices](#-best-practices)
- [Monitoring & Observability](#-monitoring--observability)
- [Security](#-security)
- [Privacy](#-privacy)
- [Troubleshooting](#-troubleshooting)
- [Contributing](#-contributing)
- [Roadmap](#-roadmap)
- [License](#-license)
- [Support](#-support)

---

## 🎯 Problem Statement

Teams deploying Kubernetes in production face critical challenges:

- **Configuration Complexity**: Kubernetes has 50+ resource types with intricate relationships and dependencies
- **Security Vulnerabilities**: Default configurations lack essential security controls like network policies and pod security standards
- **Reliability Issues**: Applications fail under load due to improper resource limits, missing health checks, and poor autoscaling
- **Operational Overhead**: Managing deployments, rollbacks, and scaling manually is error-prone and time-consuming
- **Inconsistent Environments**: Dev/staging/prod drift leads to "works on my machine" scenarios
- **Knowledge Gap**: Production-grade Kubernetes requires deep expertise in networking, storage, security, and observability
- **Cost Overruns**: Poor resource management leads to over-provisioning and wasted cloud spend

---

## 💡 Solution

Kubernetes Production Patterns provides **proven, production-ready configurations and Helm charts** that enable teams to:

✅ Deploy cloud-native applications with production-grade reliability from day one  
✅ Implement security best practices including RBAC, network policies, and pod security  
✅ Achieve high availability with proper resource management and autoscaling  
✅ Standardize deployments across all environments with GitOps workflows  
✅ Monitor and troubleshoot applications with integrated observability  
✅ Accelerate Kubernetes adoption with comprehensive examples and documentation  
✅ Reduce operational costs through optimized resource allocation

---

## ✨ Features

### Production-Ready Patterns

- **🚀 Deployment Patterns**: Blue-green, canary, rolling updates, and progressive delivery
- **📊 Autoscaling**: HPA (Horizontal Pod Autoscaler), VPA (Vertical Pod Autoscaler), and cluster autoscaling
- **🔐 Security**: NetworkPolicies, PodSecurityPolicies, RBAC, and secret management
- **💾 Storage**: StatefulSets with persistent volumes and dynamic provisioning
- **🌐 Networking**: Ingress controllers, service mesh integration, and DNS configurations
- **📈 Observability**: Prometheus metrics, logging, tracing, and Grafana dashboards
- **🔄 GitOps**: ArgoCD and Flux deployment workflows
- **⚡ Performance**: Resource optimization and cost-efficient configurations
- **🔧 Self-Healing**: Liveness/readiness probes, pod disruption budgets, and restart policies

### Helm Charts Library

- **Microservices**: REST APIs, gRPC services, message queues
- **Databases**: PostgreSQL, MySQL, MongoDB, Redis, Cassandra
- **Message Brokers**: Kafka, RabbitMQ, NATS, Pulsar
- **Caching**: Redis, Memcached, Hazelcast
- **Web Applications**: Node.js, Python, Java, Go, .NET
- **CI/CD Tools**: Jenkins, GitLab Runner, Tekton, Argo Workflows

---

## 🛠️ Technology Stack

| Category | Technologies |
|----------|-------------|
| **Orchestration** | Kubernetes 1.28+, kubectl, kubeadm |
| **Package Management** | Helm 3.0+, Kustomize |
| **GitOps** | ArgoCD, Flux CD |
| **Service Mesh** | Istio, Linkerd, Consul |
| **Ingress** | NGINX Ingress, Traefik, Kong, HAProxy |
| **Storage** | Rook-Ceph, Longhorn, OpenEBS, AWS EBS, Azure Disk, GCP PD |
| **Monitoring** | Prometheus, Grafana, Alertmanager, Thanos |
| **Logging** | Loki, Elasticsearch, Fluentd, Fluent Bit |
| **Tracing** | Jaeger, Zipkin, OpenTelemetry, Tempo |
| **Security** | OPA Gatekeeper, Falco, Trivy, Kyverno, Vault |
| **CI/CD** | Tekton, GitHub Actions, GitLab CI, Jenkins X |
| **Load Testing** | k6, Locust, JMeter |

---

## 🏛️ Architecture

### High-Level Kubernetes Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster                        │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │                  Ingress Layer                      │    │
│  │  [NGINX Ingress] → [Service Mesh (Istio)]         │    │
│  │  [TLS Termination] → [Rate Limiting]              │    │
│  └──────────────────────┬─────────────────────────────┘    │
│                         │                                   │
│  ┌──────────────────────┴─────────────────────────────┐    │
│  │              Application Layer                      │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐         │    │
│  │  │  Pod     │  │  Pod     │  │  Pod     │         │    │
│  │  │  (API)   │  │  (Web)   │  │ (Worker) │         │    │
│  │  │  + HPA   │  │  + HPA   │  │  + VPA   │         │    │
│  │  └────┬─────┘  └────┬─────┘  └────┬─────┘         │    │
│  │       │             │             │                │    │
│  │  [ConfigMap] [Secrets] [ServiceAccount]           │    │
│  └──────────────────────┬─────────────────────────────┘    │
│                         │                                   │
│  ┌──────────────────────┴─────────────────────────────┐    │
│  │              Data Layer                             │    │
│  │  ┌──────────┐  ┌──────────┐  ┌──────────┐         │    │
│  │  │StatefulSet│ │ PVC      │  │  Redis   │         │    │
│  │  │(Postgres) │ │ (Storage)│  │  (Cache) │         │    │
│  │  │Multi-AZ   │ │ Encrypted│  │ Sentinel │         │    │
│  │  └──────────┘  └──────────┘  └──────────┘         │    │
│  └────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │           Observability Layer                       │    │
│  │  [Prometheus] [Grafana] [Loki] [Jaeger] [Falco]   │    │
│  └────────────────────────────────────────────────────┘    │
│                                                              │
│  ┌────────────────────────────────────────────────────┐    │
│  │              Security Layer                         │    │
│  │  [OPA] [Network Policies] [Pod Security] [Vault]   │    │
│  └────────────────────────────────────────────────────┘    │
└─────────────────────────────────────────────────────────────┘
```

---

## 📋 Prerequisites

Before using this project, ensure you have:

- **Kubernetes Cluster** (v1.28 or higher)
  - **Managed**: EKS, AKS, GKE, or
  - **Self-hosted**: kubeadm, k3s, RKE, or
  - **Local**: kind, minikube, k3d (for development)
- **kubectl** v1.28+ ([Installation Guide](https://kubernetes.io/docs/tasks/tools/))
- **Helm** 3.0+ ([Installation Guide](https://helm.sh/docs/intro/install/))
- **Git** for version control
- **Basic Kubernetes knowledge**: Pods, Deployments, Services, ConfigMaps

### Cluster Requirements

- **Minimum**: 3 nodes, 8GB RAM per node, 4 vCPUs per node
- **Recommended**: 5+ nodes, 16GB RAM per node, 8 vCPUs per node
- **Storage**: Dynamic provisioning enabled (StorageClass)
- **Networking**: CNI plugin installed (Calico, Flannel, Cilium)

### Recommended Tools

- **k9s**: Terminal UI for Kubernetes cluster management
- **kubectx/kubens**: Context and namespace switcher
- **stern**: Multi-pod log tailing
- **kustomize**: Template-free Kubernetes configuration
- **dive**: Docker image layer analyzer
- **kubeval**: Kubernetes manifest validator

---

## 📥 Installation

### 1. Clone the Repository

```bash
git clone https://github.com/cloud-premises/kubernetes-production-patterns.git
cd kubernetes-production-patterns
```

### 2. Verify Cluster Access

```bash
# Check cluster connection
kubectl cluster-info

# Verify nodes are ready
kubectl get nodes

# Check current context
kubectl config current-context

# View cluster resources
kubectl get all --all-namespaces
```

### 3. Install Required Tools

```bash
# Install Helm (if not already installed)
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash

# Verify Helm installation
helm version

# Install kubectl (if needed)
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

### 4. Add Helm Repositories

```bash
# Add common Helm repositories
helm repo add stable https://charts.helm.sh/stable
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add jetstack https://charts.jetstack.io
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
helm repo update
```

### 5. Create Namespace

```bash
# Create dedicated namespace for patterns
kubectl create namespace production-patterns

# Set as default namespace (optional)
kubectl config set-context --current --namespace=production-patterns
```

---

## 🚀 Production Patterns

### Pattern 1: High-Availability Web Application

Complete deployment with autoscaling, health checks, and resource management.

```yaml
# patterns/web-app/deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  namespace: production
  labels:
    app: web-app
    version: v1.0.0
spec:
  replicas: 3
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 0
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
        version: v1.0.0
      annotations:
        prometheus.io/scrape: "true"
        prometheus.io/port: "8080"
        prometheus.io/path: "/metrics"
    spec:
      # Anti-affinity to spread pods across nodes
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - weight: 100
            podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app
                  operator: In
                  values:
                  - web-app
              topologyKey: kubernetes.io/hostname
      
      # Security context
      securityContext:
        runAsNonRoot: true
        runAsUser: 1000
        fsGroup: 1000
      
      containers:
      - name: app
        image: myapp:v1.0.0
        imagePullPolicy: IfNotPresent
        
        ports:
        - name: http
          containerPort: 8080
          protocol: TCP
        
        # Resource management
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
        
        # Health checks
        livenessProbe:
          httpGet:
            path: /health/live
            port: http
          initialDelaySeconds: 30
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
        
        # Graceful shutdown
        lifecycle:
          preStop:
            exec:
              command: ["/bin/sh", "-c", "sleep 15"]
        
        # Environment variables
        env:
        - name: APP_ENV
          value: "production"
        - name: LOG_LEVEL
          value: "info"
        - name: DATABASE_URL
          valueFrom:
            secretKeyRef:
              name: db-credentials
              key: url
        - name: REDIS_HOST
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: redis.host
        
        # Container security
        securityContext:
          allowPrivilegeEscalation: false
          readOnlyRootFilesystem: true
          capabilities:
            drop:
            - ALL
        
        # Volume mounts
        volumeMounts:
        - name: tmp
          mountPath: /tmp
        - name: cache
          mountPath: /app/cache
      
      volumes:
      - name: tmp
        emptyDir: {}
      - name: cache
        emptyDir: {}
---
# patterns/web-app/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app
  namespace: production
  labels:
    app: web-app
spec:
  type: ClusterIP
  ports:
  - port: 80
    targetPort: http
    protocol: TCP
    name: http
  selector:
    app: web-app
---
# patterns/web-app/hpa.yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: web-app
  namespace: production
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: web-app
  minReplicas: 3
  maxReplicas: 10
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
        value: 50
        periodSeconds: 60
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
      - type: Percent
        value: 100
        periodSeconds: 30
      - type: Pods
        value: 2
        periodSeconds: 30
      selectPolicy: Max
---
# patterns/web-app/pdb.yaml
apiVersion: policy/v1
kind: PodDisruptionBudget
metadata:
  name: web-app
  namespace: production
spec:
  minAvailable: 2
  selector:
    matchLabels:
      app: web-app
```

**Deploy:**
```bash
kubectl apply -f patterns/web-app/
```

### Pattern 2: Canary Deployment

Progressive traffic shifting for safe releases.

```yaml
# patterns/canary/deployment-stable.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-stable
  namespace: production
spec:
  replicas: 9
  selector:
    matchLabels:
      app: myapp
      version: stable
  template:
    metadata:
      labels:
        app: myapp
        version: stable
    spec:
      containers:
      - name: app
        image: myapp:v1.0.0
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
---
# patterns/canary/deployment-canary.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: app-canary
  namespace: production
spec:
  replicas: 1  # 10% of traffic
  selector:
    matchLabels:
      app: myapp
      version: canary
  template:
    metadata:
      labels:
        app: myapp
        version: canary
    spec:
      containers:
      - name: app
        image: myapp:v2.0.0
        ports:
        - containerPort: 8080
        resources:
          requests:
            memory: "256Mi"
            cpu: "250m"
          limits:
            memory: "512Mi"
            cpu: "500m"
---
# patterns/canary/service.yaml
apiVersion: v1
kind: Service
metadata:
  name: myapp
  namespace: production
spec:
  selector:
    app: myapp  # Selects both stable and canary
  ports:
  - port: 80
    targetPort: 8080
```

**Progressive Rollout:**
```bash
# Start with 10% canary
kubectl apply -f patterns/canary/

# Monitor metrics
kubectl top pods -l app=myapp
watch kubectl get pods -l app=myapp

# Increase to 50%
kubectl scale deployment app-stable --replicas=5
kubectl scale deployment app-canary --replicas=5

# Full rollout
kubectl scale deployment app-stable --replicas=0
kubectl scale deployment app-canary --replicas=10

# Cleanup old version
kubectl delete deployment app-stable
```

### Pattern 3: StatefulSet with Persistent Storage

Database deployment with ordered scaling and persistent volumes.

```yaml
# patterns/statefulset/postgresql.yaml
apiVersion: v1
kind: Service
metadata:
  name: postgres
  namespace: production
spec:
  ports:
  - port: 5432
    name: postgres
  clusterIP: None
  selector:
    app: postgres
---
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: postgres
  namespace: production
spec:
  serviceName: postgres
  replicas: 3
  selector:
    matchLabels:
      app: postgres
  template:
    metadata:
      labels:
        app: postgres
    spec:
      terminationGracePeriodSeconds: 30
      containers:
      - name: postgres
        image: postgres:15-alpine
        ports:
        - containerPort: 5432
          name: postgres
        env:
        - name: POSTGRES_PASSWORD
          valueFrom:
            secretKeyRef:
              name: postgres-secret
              key: password
        - name: PGDATA
          value: /var/lib/postgresql/data/pgdata
        resources:
          requests:
            memory: "512Mi"
            cpu: "500m"
          limits:
            memory: "1Gi"
            cpu: "1000m"
        volumeMounts:
        - name: data
          mountPath: /var/lib/postgresql/data
        livenessProbe:
          exec:
            command:
            - /bin/sh
            - -c
            - pg_isready -U postgres
          initialDelaySeconds: 30
          periodSeconds: 10
        readinessProbe:
          exec:
            command:
            - /bin/sh
            - -c
            - pg_isready -U postgres
          initialDelaySeconds: 5
          periodSeconds: 5
  volumeClaimTemplates:
  - metadata:
      name: data
    spec:
      accessModes: ["ReadWriteOnce"]
      storageClassName: fast-ssd
      resources:
        requests:
          storage: 100Gi
```

### Pattern 4: Network Policies

Microsegmentation for enhanced security.

```yaml
# patterns/network-policy/default-deny.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Ingress
  - Egress
---
# patterns/network-policy/allow-web-to-api.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-web-to-api
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: api
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: web
    ports:
    - protocol: TCP
      port: 8080
---
# patterns/network-policy/allow-api-to-database.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-api-to-database
  namespace: production
spec:
  podSelector:
    matchLabels:
      app: postgres
  policyTypes:
  - Ingress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: api
    ports:
    - protocol: TCP
      port: 5432
---
# patterns/network-policy/allow-dns.yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns
  namespace: production
spec:
  podSelector: {}
  policyTypes:
  - Egress
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: kube-system
    ports:
    - protocol: UDP
      port: 53
```

[View All Patterns →](patterns/)

---

## 📦 Helm Charts

### Available Charts

1. **web-application**: Generic web app with autoscaling
2. **api-gateway**: Kong/NGINX API gateway
3. **microservice**: REST/gRPC microservice template
4. **database**: PostgreSQL, MySQL, MongoDB
5. **cache**: Redis with Sentinel
6. **message-queue**: Kafka, RabbitMQ
7. **monitoring-stack**: Prometheus + Grafana
8. **logging-stack**: Loki + Promtail

### Using Helm Charts

```bash
# Install a chart
helm install myapp ./charts/web-application \
  --namespace production \
  --values values-production.yaml

# Upgrade a release
helm upgrade myapp ./charts/web-application \
  --namespace production \
  --values values-production.yaml

# Rollback
helm rollback myapp 1

# Uninstall
helm uninstall myapp --namespace production
```

### Example values.yaml

```yaml
# values-production.yaml
replicaCount: 3

image:
  repository: myapp
  tag: v1.0.0
  pullPolicy: IfNotPresent

resources:
  requests:
    memory: "256Mi"
    cpu: "250m"
  limits:
    memory: "512Mi"
    cpu: "500m"

autoscaling:
  enabled: true
  minReplicas: 3
  maxReplicas: 10
  targetCPUUtilizationPercentage: 70

service:
  type: ClusterIP
  port: 80

ingress:
  enabled: true
  className: nginx
  hosts:
    - host: myapp.example.com
      paths:
        - path: /
          pathType: Prefix
  tls:
    - secretName: myapp-tls
      hosts:
        - myapp.example.com

postgresql:
  enabled: true
  auth:
    username: myapp
    database: myapp

redis:
  enabled: true
  master:
    persistence:
      enabled: true
      size: 8Gi
```

[View All Charts →](charts/)

---

## 🚢 Deployment Strategies

### Rolling Update (Default)

Gradual replacement of old pods with new ones.

```yaml
spec:
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 25%
      maxUnavailable: 25%
```

**Use when:** Standard deployments, minimal downtime acceptable

### Blue-Green Deployment

Two identical environments, instant traffic switch.

```bash
# Deploy green (new version)
kubectl apply -f deployment-green.yaml

# Test green environment
kubectl port-forward svc/app-green 8080:80

# Switch traffic
kubectl patch service app -p '{"spec":{"selector":{"version":"green"}}}'

# Cleanup blue
kubectl delete deployment app-blue
```

**Use when:** Zero-downtime requirement, instant rollback needed

### Canary Deployment

Gradual traffic shift to new version.

**Use when:** Risk mitigation, gradual validation needed

### Recreate Strategy

Delete all old pods before creating new ones.

```yaml
spec:
  strategy:
    type: Recreate
```

**Use when:** Shared resources, no concurrent versions allowed

---

## 🎯 Best Practices

### Resource Management

```yaml
resources:
  requests:
    memory: "256Mi"  # Guaranteed
    cpu: "250m"      # 0.25 cores
  limits:
    memory: "512Mi"  # Maximum
    cpu: "500m"      # 0.5 cores
```

**Guidelines:**
- Set requests based on average usage
- Set limits 2x requests for burstable workloads
- Use VPA to right-size resources

### Health Checks

```yaml
livenessProbe:
  httpGet:
    path: /health/live
    port: 8080
  initialDelaySeconds: 30
  periodSeconds: 10

readinessProbe:
  httpGet:
    path: /health/ready
    port: 8080
  initialDelaySeconds: 5
  periodSeconds: 5
```

**Guidelines:**
- Liveness: Restart unhealthy pods
- Readiness: Remove from service when not ready
- Startup: For slow-starting applications

### Configuration Management

```bash
# ConfigMaps for non-sensitive data
kubectl create configmap app-config --from-file=config.yaml

# Secrets for sensitive data
kubectl create secret generic db-credentials \
  --from-literal=username=admin \
  --from-literal=password=secret123

# Use external secret managers
# - AWS Secrets Manager
# - HashiCorp Vault
# - Azure Key Vault
```

### Labels and Selectors

```yaml
metadata:
  labels:
    app: myapp
    version: v1.0.0
    environment: production
    team: backend
    cost-center: engineering
```

**Standard Labels:**
- `app.kubernetes.io/name`
- `app.kubernetes.io/instance`
- `app.kubernetes.io/version`
- `app.kubernetes.io/component`
- `app.kubernetes.io/part-of`
- `app.kubernetes.io/managed-by`

---

## 📈 Monitoring & Observability

### Prometheus Metrics

```yaml
apiVersion: v1
kind: ServiceMonitor
metadata:
  name: myapp
  namespace: production
spec:
  selector:
    matchLabels:
      app: myapp
  endpoints:
  - port: metrics
    interval: 30s
    path: /metrics
```

### Logging with Loki

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: promtail-config
data:
  promtail.yaml: |
    server:
      http_listen_port: 9080
    clients:
      - url: http://loki:3100/loki/api/v1/push
    scrape_configs:
      - job_name: kubernetes-pods
        kubernetes_sd_configs:
          - role: pod
```

### Distributed Tracing

```yaml
# OpenTelemetry Collector
apiVersion: v1
kind: ConfigMap
metadata:
  name: otel-collector-config
data:
  config.yaml: |
    receivers:
      otlp:
        protocols:
          grpc:
          http:
    exporters:
      jaeger:
        endpoint: jaeger:14250
    service:
      pipelines:
        traces:
          receivers: [otlp]
          exporters: [jaeger]
```

### Dashboards

Pre-built Grafana dashboards included:
- Kubernetes cluster overview
- Pod resource usage
- Application performance
- Network traffic analysis
- Storage metrics

---

## 🔒 Security

### Security Features

- **Pod Security Standards**: Baseline, restricted, and privileged policies
- **Network Policies**: Microsegmentation and traffic control
- **RBAC**: Role-based access control for users and service accounts
- **Secrets Management**: Integration with Vault, AWS Secrets Manager, Azure Key Vault
- **Image Scanning**: Trivy integration for vulnerability scanning
- **Runtime Security**: Falco for threat detection
- **Policy Enforcement**: OPA Gatekeeper for admission control

### Pod Security Policy

```yaml
apiVersion: policy/v1beta1
kind: PodSecurityPolicy
metadata:
  name: restricted
spec:
  privileged: false
  allowPrivilegeEscalation: false
  requiredDropCapabilities:
    - ALL
  volumes:
    - 'configMap'
    - 'emptyDir'
    - 'projected'
    - 'secret'
    - 'downwardAPI'
    - 'persistentVolumeClaim'
  runAsUser:
    rule: 'MustRunAsNonRoot'
  seLinux:
    rule: 'RunAsAny'
  fsGroup:
    rule: 'RunAsAny'
  readOnlyRootFilesystem: true
```

### RBAC Configuration

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: myapp
  namespace: production
---
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: myapp
  namespace: production
rules:
- apiGroups: [""]
  resources: ["configmaps", "secrets"]
  verbs: ["get", "list", "watch"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: myapp
  namespace: production
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: Role
  name: myapp
subjects:
- kind: ServiceAccount
  name: myapp
  namespace: production
```

### Image Scanning

```bash
# Scan container images with Trivy
trivy image myapp:v1.0.0

# Scan Kubernetes manifests
trivy k8s --report summary deployment.yaml

# Continuous scanning
kubectl apply -f https://raw.githubusercontent.com/aquasecurity/trivy-operator/main/deploy/static/trivy-operator.yaml
```

### Security Best Practices

1. **Run as non-root user**
2. **Use read-only root filesystem**
3. **Drop all capabilities**
4. **Enable network policies**
5. **Scan images for vulnerabilities**
6. **Encrypt secrets at rest**
7. **Use namespaces for isolation**
8. **Implement RBAC policies**
9. **Enable audit logging**
10. **Regular security updates**

### Vulnerability Reporting

If you discover a security vulnerability, please email: **security@cloudpremises.org**

**Do NOT** create public GitHub issues for security vulnerabilities.

**Response Timeline:**
- Initial acknowledgment: Within 24 hours
- Status updates: Every 48 hours
- Resolution target: Within 14 days for critical issues

We follow responsible disclosure and will credit security researchers who report vulnerabilities responsibly.

---

## 🔐 Privacy

### Data Collection

This project does **NOT** collect, store, or transmit any personal or sensitive data. All configurations are deployed in your Kubernetes cluster under your complete control.

### Third-Party Services

The only external services potentially accessed are:
- **Container Registries**: Docker Hub, Quay.io, or private registries (required)
- **Helm Chart Repositories**: For dependency management (optional)
- **Monitoring Backends**: If configured (Grafana Cloud, Datadog, etc.)

### Data Sovereignty

- All workloads run in your Kubernetes cluster
- No data leaves your infrastructure by default
- You control all observability and logging endpoints
- Full compliance with data residency requirements

### Compliance Support

Our patterns support compliance with:
- **GDPR** (General Data Protection Regulation)
- **HIPAA** (Health Insurance Portability and Accountability Act)
- **PCI-DSS** (Payment Card Industry Data Security Standard)
- **SOC 2** Type II
- **ISO 27001**
- **FedRAMP** (Federal Risk and Authorization Management Program)

### Telemetry

No telemetry or usage data is collected by this project. All metrics and logs remain in your infrastructure.

---

## 🐛 Troubleshooting

### Common Issues

**Issue**: Pods stuck in `Pending` state
```bash
# Check pod events
kubectl describe pod <pod-name>

# Common causes:
# - Insufficient resources
kubectl top nodes
kubectl describe nodes

# - No available storage
kubectl get pvc
kubectl get storageclass

# - Pod affinity/anti-affinity conflicts
kubectl get pod <pod-name> -o yaml | grep -A 10 affinity
```

**Issue**: Pods in `CrashLoopBackOff`
```bash
# Check logs
kubectl logs <pod-name> --previous

# Check events
kubectl describe pod <pod-name>

# Common causes:
# - Application crashes
# - Missing dependencies
# - Incorrect environment variables
# - Health check failures
```

**Issue**: Service not accessible
```bash
# Check service endpoints
kubectl get endpoints <service-name>

# Verify pod labels match service selector
kubectl get pods --show-labels
kubectl describe service <service-name>

# Test from within cluster
kubectl run test --rm -it --image=busybox -- wget -O- http://<service-name>
```

**Issue**: High memory usage / OOMKilled
```bash
# Check resource limits
kubectl describe pod <pod-name> | grep -A 5 Limits

# View actual usage
kubectl top pod <pod-name>

# Solution: Increase memory limits or optimize application
kubectl set resources deployment <name> -c=<container> --limits=memory=1Gi
```

**Issue**: ImagePullBackOff
```bash
# Check image pull errors
kubectl describe pod <pod-name>

# Common causes:
# - Image doesn't exist
# - Wrong image name/tag
# - Private registry authentication
# - Rate limiting

# Create image pull secret
kubectl create secret docker-registry regcred \
  --docker-server=<registry-url> \
  --docker-username=<username> \
  --docker-password=<password>
```

**Issue**: Network policy blocking traffic
```bash
# List network policies
kubectl get networkpolicies

# Describe policy
kubectl describe networkpolicy <policy-name>

# Temporarily disable for testing
kubectl delete networkpolicy <policy-name>

# Test connectivity
kubectl run test --rm -it --image=busybox -- nc -zv <service> <port>
```

### Debugging Commands

```bash
# Get detailed pod information
kubectl get pod <pod-name> -o yaml

# Execute commands in container
kubectl exec -it <pod-name> -- /bin/sh

# Port forward for local testing
kubectl port-forward pod/<pod-name> 8080:8080

# View cluster events
kubectl get events --sort-by='.lastTimestamp'

# Check resource usage
kubectl top nodes
kubectl top pods --all-namespaces

# View logs with timestamps
kubectl logs <pod-name> --timestamps=true

# Stream logs from multiple pods
kubectl logs -f -l app=myapp

# Check API server logs
kubectl logs -n kube-system kube-apiserver-<node>
```

### Performance Optimization

```bash
# Analyze resource requests vs limits
kubectl describe nodes | grep -A 5 "Allocated resources"

# Find pods with no resource limits
kubectl get pods --all-namespaces -o json | \
  jq '.items[] | select(.spec.containers[].resources.limits == null) | .metadata.name'

# Identify pods using most resources
kubectl top pods --all-namespaces --sort-by=memory
kubectl top pods --all-namespaces --sort-by=cpu
```

[View Full Troubleshooting Guide →](docs/TROUBLESHOOTING.md)

---

## 🤝 Contributing

We welcome contributions from the community! Here's how you can help:

### Ways to Contribute

- 🐛 Report bugs and issues
- 💡 Suggest new patterns or improvements
- 📖 Improve documentation
- 🔧 Submit bug fixes
- ✨ Add new deployment patterns
- 📊 Create Helm charts
- 🧪 Write tests
- 🎨 Share production experiences

### How to Contribute

1. **Fork** the repository
2. **Create** a feature branch (`git checkout -b feature/new-pattern`)
3. **Commit** your changes (`git commit -m 'Add canary deployment pattern'`)
4. **Push** to the branch (`git push origin feature/new-pattern`)
5. **Open** a Pull Request

### Contribution Guidelines

- Follow Kubernetes best practices
- Include comprehensive documentation
- Add examples for new patterns
- Test configurations in a real cluster
- Update relevant documentation
- Sign commits with GPG key (recommended)

### Pattern Submission Checklist

- [ ] Pattern addresses a real production use case
- [ ] Includes complete YAML manifests
- [ ] Has deployment instructions
- [ ] Includes monitoring/observability configuration
- [ ] Documents resource requirements
- [ ] Provides troubleshooting guide
- [ ] Tested in at least one environment

### Development Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/kubernetes-production-patterns.git
cd kubernetes-production-patterns

# Add upstream remote
git remote add upstream https://github.com/cloud-premises/kubernetes-production-patterns.git

# Create local cluster for testing
kind create cluster --name test-patterns

# Set context
kubectl cluster-info --context kind-test-patterns

# Test pattern
kubectl apply -f patterns/your-pattern/

# Validate
kubectl get all -n your-namespace
```

### Code Review Process

1. Automated checks run on all PRs
2. Pattern validation in test cluster
3. Documentation review
4. Maintainer review within 48-72 hours
5. Address feedback
6. Two approvals required for merge
7. Squash and merge to main

### Commit Message Format

Follow [Conventional Commits](https://www.conventionalcommits.org/):

```
feat: add blue-green deployment pattern
fix: correct resource limits in web-app pattern
docs: update installation instructions
test: add validation for StatefulSet pattern
chore: update Helm chart dependencies
```

[Read Full Contributing Guide →](CONTRIBUTING.md)

---

## 🗺️ Roadmap

### Current Focus (Q4 2024 - Q1 2025)

- ✅ High-availability deployment patterns
- ✅ Autoscaling configurations (HPA, VPA)
- ✅ StatefulSet patterns with persistent storage
- ✅ Network policy templates
- ✅ Security best practices
- 🚧 Service mesh integration (Istio, Linkerd)
- 🚧 GitOps patterns (ArgoCD, Flux)
- 🚧 Progressive delivery with Flagger

### Upcoming (Q2 2025)

- 📋 Multi-cluster deployments
- 📋 Disaster recovery patterns
- 📋 Cost optimization strategies
- 📋 AI/ML workload patterns
- 📋 Serverless Kubernetes (Knative)
- 📋 Advanced monitoring dashboards
- 📋 Chaos engineering patterns

### Future Enhancements (2025)

- Multi-cloud Kubernetes patterns
- Edge computing deployments
- IoT workload patterns
- Batch processing frameworks
- Hybrid cloud configurations
- Kubernetes operator development guide
- Performance tuning playbook

### Community Requests

Vote on features at: [GitHub Discussions](https://github.com/cloud-premises/kubernetes-production-patterns/discussions)

[View Detailed Roadmap →](ROADMAP.md)

---

## 📄 License

This project is licensed under the **Apache License 2.0** - see the [LICENSE](LICENSE) file for details.

```
Copyright 2025 Cloud Premises

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
```

### Third-Party Licenses

This project includes patterns and examples for various open-source projects:
- Kubernetes (Apache 2.0)
- Helm (Apache 2.0)
- Prometheus (Apache 2.0)
- Grafana (AGPL 3.0)

See [THIRD_PARTY_LICENSES.md](THIRD_PARTY_LICENSES.md) for complete details.

---

## 💬 Support

### Community Support (Free)

- 📧 **Email**: opensource@cloudpremises.org
- 💬 **Discord**: [Join our community](https://discord.gg/cloudpremises)
- 📖 **Documentation**: [docs.cloudpremises.com](https://docs.cloudpremises.com)
- 🐛 **Issue Tracker**: [GitHub Issues](https://github.com/cloud-premises/kubernetes-production-patterns/issues)
- 💡 **Discussions**: [GitHub Discussions](https://github.com/cloud-premises/kubernetes-production-patterns/discussions)
- 🎥 **YouTube**: [Tutorials and walkthroughs](https://youtube.com/@cloudpremises)

### Professional Support (Paid)

For enterprise support, consulting, or custom pattern development:
- 📧 Email: consulting@cloudpremises.com
- 🌐 Website: [cloudpremises.org](https://cloudpremises.org)
- 📞 Schedule: [Book a consultation](https://calendly.com/cloudpremises)

**Enterprise Support Includes:**
- 24/7 critical issue support
- Dedicated Slack/Teams channel
- Custom pattern development
- Architecture review and optimization
- On-site/remote training workshops
- Migration assistance
- SLA-backed response times

### Response Times (Community)

- **Critical Production Issues**: 24 hours
- **Bug Reports**: 2-3 business days
- **Pattern Requests**: 1 week
- **Questions**: 3-5 business days

### Training & Workshops

We offer:
- Kubernetes fundamentals
- Production deployment strategies
- Security best practices
- Observability and monitoring
- GitOps workflows
- Custom team training

Contact: training@cloudpremises.org

---

## 🙏 Acknowledgments

This project wouldn't be possible without:

- **Kubernetes Community** for the amazing platform
- **CNCF** for fostering cloud-native technologies
- **Helm Community** for package management
- **All Contributors** who have shared patterns and improvements
- **Production Users** providing real-world feedback

### Built With

- [Kubernetes](https://kubernetes.io/) - Container Orchestration
- [Helm](https://helm.sh/) - Package Manager
- [Prometheus](https://prometheus.io/) - Monitoring
- [Grafana](https://grafana.com/) - Visualization
- [ArgoCD](https://argoproj.github.io/cd/) - GitOps
- [Istio](https://istio.io/) - Service Mesh

### Inspired By

- Kubernetes Official Documentation
- Google's Site Reliability Engineering practices
- Netflix's container orchestration patterns
- Weaveworks' GitOps methodology
- CNCF best practices

### Contributors

Thanks to all our contributors! 🎉

<a href="https://github.com/cloud-premises/kubernetes-production-patterns/graphs/contributors">
  <img src="https://contrib.rocks/image?repo=cloud-premises/kubernetes-production-patterns" />
</a>

---

## 📊 Project Stats

![GitHub stars](https://img.shields.io/github/stars/cloud-premises/kubernetes-production-patterns?style=social)
![GitHub forks](https://img.shields.io/github/forks/cloud-premises/kubernetes-production-patterns?style=social)
![GitHub issues](https://img.shields.io/github/issues/cloud-premises/kubernetes-production-patterns)
![GitHub pull requests](https://img.shields.io/github/issues-pr/cloud-premises/kubernetes-production-patterns)
![Last commit](https://img.shields.io/github/last-commit/cloud-premises/kubernetes-production-patterns)
![Contributors](https://img.shields.io/github/contributors/cloud-premises/kubernetes-production-patterns)

---

## 🌟 Stargazers Over Time

[![Stargazers over time](https://starchart.cc/cloud-premises/kubernetes-production-patterns.svg)](https://starchart.cc/cloud-premises/kubernetes-production-patterns)

---

## 📚 Additional Resources

### Official Documentation
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Helm Documentation](https://helm.sh/docs/)
- [CNCF Landscape](https://landscape.cncf.io/)

### Recommended Reading
- [Kubernetes Best Practices](https://kubernetes.io/docs/concepts/configuration/overview/)
- [The Twelve-Factor App](https://12factor.net/)
- [Site Reliability Engineering](https://sre.google/books/)
- [Production Kubernetes](https://www.oreilly.com/library/view/production-kubernetes/9781492092292/)

### Community
- [Kubernetes Slack](https://kubernetes.slack.com/)
- [CNCF Slack](https://cloud-native.slack.com/)
- [r/kubernetes](https://reddit.com/r/kubernetes)
- [Kubernetes Blog](https://kubernetes.io/blog/)

---

<div align="center">

**Made with ❤️ by [Cloud Premises](https://cloudpremises.org)**

⭐ If this project helps you, please star it on GitHub!

[Website](https://cloudpremises.org) • [Documentation](https://docs.cloudpremises.com) • [Blog](https://blog.cloudpremises.com) • [Twitter](https://twitter.com/cloudpremises)

*Empowering teams to run Kubernetes confidently in production*

</div>
