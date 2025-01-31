# 🥷 ninja-monitor

✅ Supports Minikube, Kind, and AKS\
✅ Uses kube-prometheus-stack (Prometheus, Grafana, Alertmanager)\
✅ Includes Loki for logs\
✅ CI/CD GitHub Actions to check linting & versions\
✅ GitOps Ready with FluxCD (Optional)\
✅ Deployable via Helm Repository\

## 📘 Monitoring Stack (kube-prometheus-stack + Loki)

This Helm chart deploys kube-prometheus-stack (Prometheus, Grafana, Alertmanager) and Loki for logging.

### 🚀 Installation Methods

Choose one of the following installation methods.

1️⃣ Install Using Helm
```
helm repo add my-repo https://yourgithub.io/helm-charts
helm repo update
helm install monitoring my-repo/monitoring-stack -n monitoring --create-namespace
```

2️⃣ Install from Local Chart
```
helm install monitoring ./monitoring-stack -n monitoring --create-namespace
```

3️⃣ Install Using kubectl\
Generate Kubernetes manifests and apply them directly:
```
helm template monitoring ./monitoring-stack --namespace monitoring | kubectl apply -f -
```

4️⃣ Deploy with FluxCD (HelmRelease)\
If using FluxCD, apply the HelmRelease manifest:
```
apiVersion: helm.toolkit.fluxcd.io/v2beta1
kind: HelmRelease
metadata:
  name: monitoring-stack
  namespace: monitoring
spec:
  chart:
    spec:
      chart: monitoring-stack
      sourceRef:
        kind: HelmRepository
        name: my-repo
  interval: 5m
  values:
    grafana:
      adminPassword: "supersecret"
```
Apply it:
```
kubectl apply -f helmrelease.yaml
```

📡 Accessing Grafana
```
kubectl port-forward svc/monitoring-grafana 3000:80 -n monitoring
```
  - URL: http://localhost:3000
  - User: admin
  - Password: supersecret (change in values.yaml)

\
📌 Updating the Chart
```
helm repo update
helm upgrade monitoring my-repo/monitoring-stack -n monitoring
```
