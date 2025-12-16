
# IBM HCC Control Plane - Security & Compliance Documentation

## Overview

This document outlines the security architecture, compliance measures, and security best practices implemented in the IBM Consulting Hybrid Cloud Console (HCC) Control Plane. It serves as a comprehensive guide for security teams, auditors, and compliance officers.

## Security Architecture

### Defense in Depth Strategy

```mermaid
graph TB
    subgraph "External Layer"
        WAF[Web Application Firewall]
        DDoS[DDoS Protection]
        CDN[Content Delivery Network]
    end
    
    subgraph "Network Layer"
        VPN[VPN Access]
        FW[Network Firewall]
        NP[Network Policies]
    end
    
    subgraph "Platform Layer"
        RBAC[Role-Based Access Control]
        PSP[Pod Security Policies]
        SA[Service Accounts]
    end
    
    subgraph "Application Layer"
        AuthN[Authentication]
        AuthZ[Authorization]
        Enc[Encryption]
    end
    
    subgraph "Data Layer"
        Vault[HashiCorp Vault]
        TLS[TLS Encryption]
        Backup[Encrypted Backups]
    end
    
    WAF --> FW
    DDoS --> FW
    CDN --> FW
    
    FW --> RBAC
    VPN --> RBAC
    NP --> RBAC
    
    RBAC --> AuthN
    PSP --> AuthN
    SA --> AuthN
    
    AuthN --> Vault
    AuthZ --> Vault
    Enc --> Vault
    
    style WAF fill:#ffebee
    style Vault fill:#e8f5e8
    style RBAC fill:#e1f5fe
```

### Security Zones

#### **External Zone**
- **Components**: Load Balancers, CDN, WAF
- **Security Controls**: DDoS protection, rate limiting, geographic blocking
- **Access**: Public internet (controlled)

#### **DMZ Zone**
- **Components**: Ingress Controllers, API Gateways
- **Security Controls**: TLS termination, certificate management, request filtering
- **Access**: Authenticated users only

#### **Internal Zone**
- **Components**: HCC Console, Tekton, ArgoCD
- **Security Controls**: Mutual TLS, service mesh, network policies
- **Access**: Internal services and authenticated users

#### **Secure Zone**
- **Components**: Vault, Database, Secrets
- **Security Controls**: Encryption at rest, strict RBAC, audit logging
- **Access**: Authorized services only

## Authentication & Authorization

### Identity Providers

#### GitHub SSO Integration
```yaml
# OAuth Configuration
apiVersion: v1
kind: Secret
meta
  name: github-oauth
  namespace: hcc-system
type: Opaque
stringData:
  client-id: "vault:secret/github#client_id"
  client-secret: "vault:secret/github#client_secret"
---
apiVersion: v1
kind: ConfigMap
meta
  name: auth-config
  namespace: hcc-system

  config.yaml: |
    auth:
      providers:
        github:
          development:
            clientId: ${GITHUB_CLIENT_ID}
            clientSecret: ${GITHUB_CLIENT_SECRET}
          production:
            clientId: ${GITHUB_CLIENT_ID}
            clientSecret: ${GITHUB_CLIENT_SECRET}
      session:
        secret: "vault:secret/session#secret"
```

#### Multi-Factor Authentication (MFA)
- **Requirement**: MFA mandatory for all users
- **Methods**: TOTP, Hardware tokens, SMS (fallback)
- **Integration**: GitHub organization MFA enforcement
- **Bypass**: Not permitted for any user role

### Role-Based Access Control (RBAC)

#### User Roles Matrix

| Role | HCC Console | Vault | Tekton | ArgoCD | OpenShift |
|------|-------------|-------|--------|--------|-----------|
| **Developer** | Read/Write Templates | Read Secrets | Trigger Pipelines | View Apps | View Resources |
| **Platform Engineer** | Full Access | Manage Policies | Full Pipeline Access | Manage Apps | Namespace Admin |
| **Operations** | Read-Only | Emergency Access | Monitor Pipelines | Emergency Sync | Cluster Admin |
| **Security Admin** | Audit Access | Full Access | View Logs | Read-Only | Security Admin |
| **Auditor** | Read-Only | Audit Logs | View History | Audit Logs | View-Only |

#### Kubernetes RBAC Configuration
```yaml
# Developer Role
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
meta
  namespace: hcc-pipelines
  name: developer
rules:
- apiGroups: ["tekton.dev"]
  resources: ["pipelineruns", "taskruns"]
  verbs: ["get", "list", "create", "watch"]
- apiGroups: [""]
  resources: ["pods", "pods/log"]
  verbs: ["get", "list", "watch"]
---
# Platform Engineer Role
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRole
meta
  name: platform-engineer
rules:
- apiGroups: ["tekton.dev"]
  resources: ["*"]
  verbs: ["*"]
- apiGroups: ["argoproj.io"]
  resources: ["applications", "appprojects"]
  verbs: ["get", "list", "create", "update", "patch", "delete"]
- apiGroups: [""]
  resources: ["secrets", "configmaps"]
  verbs: ["get", "list", "create", "update"]
```

### Service Account Management
```yaml
# HCC Console Service Account
apiVersion: v1
kind: ServiceAccount
meta
  name: hcc-console
  namespace: hcc-system
  annotations:
    vault.hashicorp.com/role: "hcc-console"
automountServiceAccountToken: true
---
# Vault Auth Configuration
apiVersion: v1
kind: ConfigMap
meta
  name: vault-auth
  namespace: hcc-system

  config.hcl: |
    path "auth/kubernetes" {
      type = "kubernetes"
      config = {
        kubernetes_host = "https://kubernetes.default.svc"
        kubernetes_ca_cert = "@/var/run/secrets/kubernetes.io/serviceaccount/ca.crt"
        issuer = "kubernetes/serviceaccount"
      }
    }
```

## Secret Management

### HashiCorp Vault Architecture

#### Vault Configuration
```hcl
# vault.hcl
cluster_name = "hcc-vault-cluster"
storage "raft" {
  path = "/vault/data"
  node_id = "vault-0"
}

listener "tcp" {
  address = "0.0.0.0:8200"
  tls_cert_file = "/vault/tls/tls.crt"
  tls_key_file = "/vault/tls/tls.key"
  tls_min_version = "tls12"
}

seal "awskms" {
  region = "eu-central-1"
  kms_key_id = "alias/vault-hcc-unseal"
}

api_addr = "https://vault.hcc.eu-de-sbx.cloudaccelerator.ibm.com"
cluster_addr = "https://vault-internal:8201"

ui = true
log_level = "INFO"

telemetry {
  prometheus_retention_time = "30s"
  disable_hostname = true
}
```

#### Secret Engines
```bash
# Key-Value Secrets (Application Configs)
vault secrets enable -path=secret kv-v2

# Database Dynamic Secrets
vault secrets enable database
vault write database/config/postgresql \
    plugin_name=postgresql-database-plugin \
    connection_url="postgresql://{{username}}:{{password}}@postgres:5432/hcc?sslmode=require" \
    allowed_roles="hcc-console,tekton-pipelines" \
    username="vault-admin" \
    password="vault:secret/database#admin_password"

# PKI for TLS Certificates
vault secrets enable -path=pki pki
vault secrets tune -max-lease-ttl=8760h pki
vault write pki/root/generate/internal \
    common_name="HCC Internal CA" \
    ttl=8760h

# Transit Engine for Encryption as a Service
vault secrets enable transit
vault write -f transit/keys/hcc-encryption
```

#### Vault Policies
```hcl
# HCC Console Policy
path "secret/data/hcc/console/*" {
  capabilities = ["read"]
}

path "database/creds/hcc-console" {
  capabilities = ["read"]
}

path "pki/issue/hcc-console" {
  capabilities = ["create", "update"]
}

path "transit/encrypt/hcc-encryption" {
  capabilities = ["update"]
}

path "transit/decrypt/hcc-encryption" {
  capabilities = ["update"]
}

# Tekton Pipelines Policy
path "secret/data/hcc/pipelines/*" {
  capabilities = ["read"]
}

path "database/creds/tekton-pipelines" {
  capabilities = ["read"]
}

path "secret/data/registry/*" {
  capabilities = ["read"]
}

# Operations Team Policy
path "secret/data/hcc/*" {
  capabilities = ["read", "list"]
}

path "sys/health" {
  capabilities = ["read"]
}

path "sys/seal-status" {
  capabilities = ["read"]
}
```

### Secret Rotation Strategy

#### Automated Rotation
```yaml
apiVersion: batch/v1
kind: CronJob
meta
  name: secret-rotation
  namespace: hcc-system
spec:
  schedule: "0 2 * * 0"  # Weekly on Sunday at 2 AM
  jobTemplate:
    spec:
      template:
        spec:
          serviceAccountName: vault-rotator
          containers:
          - name: rotator
            image: vault:latest
            env:
            - name: VAULT_ADDR
              value: "https://vault.hcc.eu-de-sbx.cloudaccelerator.ibm.com"
            - name: VAULT_TOKEN
              valueFrom:
                secretKeyRef:
                  name: vault-rotator-token
                  key: token
            script: |
              #!/bin/sh
              
              # Rotate database credentials
              vault write -force database/rotate-root/postgresql
              
              # Rotate API keys
              vault kv patch secret/hcc/api-keys \
                github_token="$(generate_github_token)" \
                concert_key="$(generate_concert_key)"
              
              # Rotate TLS certificates
              vault write pki/tidy tidy_cert_store=true
              
              # Notify rotation completion
              curl -X POST "$WEBHOOK_URL" \
                -H "Content-Type: application/json" \
                -d '{"message": "Secret rotation completed successfully"}'
          restartPolicy: OnFailure
```

## Encryption

### Encryption at Rest

#### Database Encryption
```yaml
# PostgreSQL with TDE
apiVersion: apps/v1
kind: StatefulSet
meta
  name: postgresql
spec:
  template:
    spec:
      containers:
      - name: postgresql
        image: postgres:15-alpine
        env:
        - name: POSTGRES_TDE_KEY
          valueFrom:
            secretKeyRef:
              name: postgres-tde-key
              key: key
        - name: POSTGRES_INITDB_ARGS
          value: "--data-checksums --auth-local=trust --auth-host=scram-sha-256"
        volumeMounts:
        - name: postgres-data
          mountPath: /var/lib/postgresql/data
          encrypted: true
        - name: tde-config
          mountPath: /etc/postgresql/postgresql.conf
          subPath: postgresql.conf
      volumes:
      - name: tde-config
        configMap:
          name: postgres-tde-config
```

#### Storage Encryption
```yaml
# Encrypted Storage Class
apiVersion: storage.k8s.io/v1
kind: StorageClass
meta
  name: encrypted-ssd
provisioner: kubernetes.io/aws-ebs
parameters:
  type: gp3
  encrypted: "true"
  kmsKeyId: "alias/hcc-storage-encryption"
  fsType: ext4
allowVolumeExpansion: true
volumeBindingMode: WaitForFirstConsumer
```

### Encryption in Transit

#### TLS Configuration
```yaml
# Ingress with TLS
apiVersion: networking.k8s.io/v1
kind: Ingress
meta
  name: hcc-console-ingress
  annotations:
    cert-manager.io/cluster-issuer: "letsencrypt-prod"
    nginx.ingress.kubernetes.io/ssl-redirect: "true"
    nginx.ingress.kubernetes.io/force-ssl-redirect: "true"
    nginx.ingress.kubernetes.io/ssl-protocols: "TLSv1.2 TLSv1.3"
    nginx.ingress.kubernetes.io/ssl-ciphers: "ECDHE-ECDSA-AES128-GCM-SHA256,ECDHE-RSA-AES128-GCM-SHA256"
spec:
  tls:
  - hosts:
    - console.hcc.eu-de-sbx.cloudaccelerator.ibm.com
    secretName: hcc-console-tls
  rules:
  - host: console.hcc.eu-de-sbx.cloudaccelerator.ibm.com
    http:
      paths:
      - path: /
        pathType: Prefix
        backend:
          service:
            name: hcc-console-service
            port:
              number: 80
```

#### Service Mesh mTLS
```yaml
# Istio PeerAuthentication
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
meta
  name: default
  namespace: hcc-system
spec:
  mtls:
    mode: STRICT
---
# Destination Rule
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
meta
  name: hcc-services
  namespace: hcc-system
spec:
  host: "*.hcc-system.svc.cluster.local"
  trafficPolicy:
    tls:
      mode: ISTIO_MUTUAL
```

## Network Security

### Network Policies

#### Ingress Controls
```yaml
# HCC Console Network Policy
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
meta
  name: hcc-console-policy
  namespace: hcc-system
spec:
  podSelector:
    matchLabels:
      app: hcc-console
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          name: istio-system
    - podSelector:
        matchLabels:
          app: istio-proxy
    ports:
    - protocol: TCP
      port: 7007
  egress:
  - to:
    - namespaceSelector:
        matchLabels:
          name: vault-system
    ports:
    - protocol: TCP
      port: 8200
  - to:
    - namespaceSelector:
        matchLabels:
          name: hcc-system
    - podSelector:
        matchLabels:
          app: postgresql
    ports:
    - protocol: TCP
      port: 5432
  - to: []
    ports:
    - protocol: TCP
      port: 443  # HTTPS outbound
    - protocol: TCP
      port: 53   # DNS
    - protocol: UDP