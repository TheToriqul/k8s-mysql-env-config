# MySQL Pod Environment Variables - Command Reference Guide

### Project Content Table

- [Section 1: Core Project Workflow](#section-1-core-project-workflow)
- [Section 2: Advanced Operations](#section-2-advanced-operations)
- [Section 3: Production Guide](#section-3-production-guide)

> **Author**: [Md Toriqul Islam](https://linkedin.com/in/TheToriqul)  
> **Description**: Command reference for managing MySQL database environment variables in Kubernetes  
> **Learning Focus**: Environment variable configuration for containerized MySQL  
> **Note**: Review and understand each command before execution.

## Section 1: Core Project Workflow

### Step 1: Create MySQL Pod (Imperative Approach)

```bash
# Create MySQL pod with environment variables
kubectl run my-db --image=mysql:latest \
    --env="MYSQL_ROOT_PASSWORD=abc123" \
    --env="MYSQL_USER=user1" \
    --env="MYSQL_PASSWORD=user1@mydb"

# Verify pod creation
kubectl get pod my-db

# Check pod status
kubectl describe pod my-db

# Get the yaml configuration file
kubectl get pod my-db -o yaml > my-db.yaml
```

### Step 2: Create MySQL Pod (Declarative Approach)

```bash
# Create pod using YAML definition
kubectl create -f env.yaml

# View pod details
kubectl describe pod my-db
```

### Step 3: Verify Environment Variables

```bash
# Access pod shell
kubectl exec -it my-db -- sh

# View environment variables
env

# Exit pod shell
exit
```

### Final Step: Cleanup

```bash
# Remove MySQL pod
kubectl delete pod my-db

# Verify pod removal
kubectl get pods
```

## Section 2: Advanced Operations

### Pod Environment Verification

```bash
# View pod logs
kubectl logs my-db

# Check specific environment variable
kubectl exec my-db -- env | grep MYSQL_USER

# Monitor pod status
kubectl get pod my-db --watch
```

### Database Access Verification

```bash
# Access MySQL shell
kubectl exec -it my-db -- mysql -u user1 -p

# View MySQL version (from MySQL shell)
SELECT VERSION();

# Exit MySQL shell
exit
```

## Section 3: Production Guide

### Production Setup

```bash
# Create production namespace
kubectl create namespace prod-db

# Deploy MySQL pod in production namespace
kubectl run my-db --image=mysql:latest \
    --env="MYSQL_ROOT_PASSWORD=abc123" \
    --env="MYSQL_USER=user1" \
    --env="MYSQL_PASSWORD=user1@mydb" \
    -n prod-db
```

### Monitoring

```bash
# Monitor pod health
kubectl get pod my-db -n prod-db -w

# View pod details
kubectl describe pod my-db -n prod-db

# Check pod logs
kubectl logs -f my-db -n prod-db
```

### Backup Operations

```bash
# Export pod definition
kubectl get pod my-db -o yaml > mysql-pod-backup.yaml

# Export logs
kubectl logs my-db > mysql-pod.log
```

## Learning Notes

1. Environment variables are crucial for MySQL configuration
2. Variables must be set before pod creation
3. Both imperative and declarative approaches are valid
4. Pod recreation is required for environment variable changes
5. Always verify environment variable propagation

---

> 💡 **Best Practice**: Use declarative YAML files for production deployments to ensure consistency and version control.

> ⚠️ **Warning**: Protect sensitive credentials in production environments.

> 📝 **Note**: Environment variables cannot be modified after pod creation without recreating the pod.
