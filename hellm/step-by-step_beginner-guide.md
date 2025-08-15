# Helm: Step-by-Step Beginner Guide
**Author**: Dr. Emmanuel N  
**Title**: Cloud/DevOps/AI Engineer  
**Date**: August 15, 2025

---

## What is Helm?
Helm is the package manager for Kubernetes. Similar to how apt (Ubuntu) or yum (CentOS) works for Linux packages, Helm simplifies the deployment, upgrade, and management of Kubernetes applications.


## Why Use Helm?
- Reusable charts across environments (dev, staging, prod)
- Parameterized configuration with `values.yaml`
- Seamless upgrade and rollback of applications
- Easy integration into DevSecOps workflows
- Access to a wide range of community-maintained charts


## Installing Helm
- On macOS: `brew install helm`
- On Linux: `curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash`
- On Windows (via Chocolatey): `choco install kubernetes-helm`


## Connect Helm to EKS
Ensure `kubectl` is configured for your EKS cluster:  
`kubectl get nodes`


## Helm Anatomy
- Chart: A Helm package containing templates and default values.  
- Values file: Custom configurations for deployments.  
- Release: A deployed instance of a chart.  
- Repository: A remote source of charts.


## Add Helm Repository
Example: Add Bitnami repo and update:  
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```


## Install a Chart (e.g., NGINX)
```
helm install my-nginx bitnami/nginx
kubectl get all
```


## Custom Configuration
Create `custom-values.yaml` with overrides:  
```
service:
  type: LoadBalancer
  port: 8080
```
Install with:  
```
helm install custom-nginx bitnami/nginx -f custom-values.yaml
```


## Upgrade and Rollback
Upgrade:  
```
helm upgrade custom-nginx bitnami/nginx -f new-values.yaml
```
Rollback:  
```
helm rollback custom-nginx 1
```


## Uninstall a Release
```
helm uninstall custom-nginx
```


## Using Namespaces
Isolate deployments:  
```
kubectl create namespace dev-tools
helm install prometheus prometheus-community/kube-prometheus-stack --namespace dev-tools
```


## Install DevSecOps and SRE Tools with Helm
**Trivy (Vulnerability scanning):**  
```
helm repo add aqua https://aquasecurity.github.io/helm-charts/
helm install trivy-operator aqua/trivy-operator --namespace trivy-system --create-namespace
```

**Checkov (IaC analysis):** Use via CI/CD pipeline or run locally:  
```
checkov -d .
```

**Grafana/Prometheus (Monitoring):**  
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm install monitoring prometheus-community/kube-prometheus-stack --namespace monitoring --create-namespace
```

**Loki (Log aggregation):**  
```
helm repo add grafana https://grafana.github.io/helm-charts
helm install loki grafana/loki-stack --namespace monitoring
```

**Kyverno (Policy enforcement):**  
```
helm repo add kyverno https://kyverno.github.io/kyverno/
helm install kyverno kyverno/kyverno --namespace kyverno --create-namespace
```

**Kubecost (Cost monitoring):**  
```
helm repo add kubecost https://kubecost.github.io/cost-analyzer/
helm install kubecost kubecost/cost-analyzer --namespace kubecost --create-namespace
```

**Falco (Runtime security):**  
```
helm repo add falcosecurity https://falcosecurity.github.io/charts
helm install falco falcosecurity/falco --namespace falco --create-namespace
```


## Validation Steps for Installed Tools
**Prometheus/Grafana:**  
```
kubectl get svc -n monitoring
kubectl port-forward svc/grafana 3000:80 -n monitoring
kubectl port-forward svc/prometheus-operated 9090:9090 -n monitoring
```
- Grafana: http://localhost:3000 (admin/admin)  
- Prometheus: http://localhost:9090  

**Trivy:**  
```
kubectl get deployments -n trivy-system
kubectl logs deploy/trivy-operator -n trivy-system
```

**Checkov:**  
```
checkov -d terraform/ --quiet
```

**Loki:**  
```
kubectl get pods -n monitoring
kubectl logs <loki-pod-name> -n monitoring
```

**Kyverno:**  
```
kubectl get cpol -A
kubectl get policies -A
```

**Kubecost:**  
```
kubectl get pods -n kubecost
kubectl port-forward --namespace kubecost svc/kubecost-cost-analyzer 9090
```

**Falco:**  
```
kubectl get pods -n falco
kubectl logs <falco-pod> -n falco
```


## Common Helm Commands Cheat Sheet
- Add repo: `helm repo add <name> <url>`  
- Update repos: `helm repo update`  
- Search charts: `helm search repo <keyword>`  
- Install: `helm install <release> <chart>`  
- Customize install: `helm install -f values.yaml`  
- Upgrade: `helm upgrade <release>`  
- Rollback: `helm rollback <release> <revision>`  
- Uninstall: `helm uninstall <release>`  
- List releases: `helm list`

