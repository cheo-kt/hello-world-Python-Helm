# Hello World Python with Helm

**Student:** Sergio Fernando Florez Sanabria  
**Student ID:** A00396046  
**Course:** Plataformas 2

---

## Overview

This project demonstrates how to deploy a simple Python Flask application to Kubernetes using Helm charts. The application responds with "Hello World from Python with Helm!" when accessed.

---

## Prerequisites

Before starting, ensure you have the following tools installed:

- **Docker** - For building container images
- **Kubernetes** - A running cluster (minikube, kind, or production cluster)
- **kubectl** - Kubernetes command-line tool
- **Helm** - Kubernetes package manager
- **minikube** (optional) - For local Kubernetes development

---

## Project Structure

```
hello-world-python-helm/
├── app.py                          # Flask application
├── requirements.txt                # Python dependencies
├── Dockerfile                      # Container image definition
└── hello-world-python-chart/       # Helm chart
    ├── Chart.yaml
    ├── values.yaml
    └── templates/
        ├── deployment.yaml
        ├── service.yaml
        ├── serviceaccount.yaml
        └── _helpers.tpl
```

---

## Step-by-Step Guide

### 1. Create the Project Directory

```bash
mkdir hello-world-python-helm
cd hello-world-python-helm
```

### 2. Create the Python Application

Create `app.py`:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def hello():
    return "Hello World from Python with Helm!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=8080)
```

Create `requirements.txt`:

```
flask==2.3.0
```

### 3. Create the Dockerfile

Create `Dockerfile`:

```dockerfile
FROM python:3.9-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install -r requirements.txt

COPY app.py .

EXPOSE 8080

CMD ["python", "app.py"]
```

### 4. Build the Docker Image

```bash
# Build the image
docker build -t hello-world-python:1.0.0 .

# If using minikube, load the image into minikube
minikube image load hello-world-python:1.0.0

# Verify the image is available
minikube image ls | grep hello-world
```

### 5. Create the Helm Chart

```bash
# Generate the base chart structure
helm create hello-world-python-chart
cd hello-world-python-chart
```

### 6. Configure values.yaml

Edit `values.yaml` with the following configuration:

```yaml
replicaCount: 1

image:
  repository: hello-world-python
  pullPolicy: IfNotPresent
  tag: "1.0.0"

imagePullSecrets: []
nameOverride: ""
fullnameOverride: ""

serviceAccount:
  create: true
  automount: true
  annotations: {}
  name: ""

podAnnotations: {}
podLabels: {}

podSecurityContext: {}
securityContext: {}

service:
  type: NodePort
  port: 8080

ingress:
  enabled: false

resources: {}

livenessProbe:
  httpGet:
    path: /
    port: http

readinessProbe:
  httpGet:
    path: /
    port: http

autoscaling:
  enabled: false

volumes: []
volumeMounts: []

nodeSelector: {}
tolerations: []
affinity: {}

httpRoute:
  enabled: false
```

### 7. Update deployment.yaml

Edit `templates/deployment.yaml` and ensure the `containerPort` is set correctly:

```yaml
ports:
  - name: http
    containerPort: 8080
    protocol: TCP
```

**Important:** Make sure there's a space after the colon: `containerPort: 8080` (not `containerPort:8080`)

### 8. Remove Unnecessary Files (Optional)

```bash
rm templates/NOTES.txt
rm templates/httproute.yaml  # If it exists
```

### 9. Deploy with Helm

Return to the parent directory and install the chart:

```bash
cd ..
helm install hello-world ./hello-world-python-chart
```

### 10. Verify the Deployment

```bash
# Check Helm release status
helm status hello-world

# View pods
kubectl get pods

# View services
kubectl get services

# List all Helm releases
helm list
```

### 11. Access the Application

If using minikube:

```bash
# Open the service in your browser
minikube service hello-world-hello-world-python-chart

# Or list all available services
minikube service list
```

Alternatively, use port forwarding:

```bash
kubectl port-forward svc/hello-world-hello-world-python-chart 8080:8080
```

Then open your browser at: `http://localhost:8080`

---

## Useful Helm Commands

```bash
# List all releases
helm list

# Get release status
helm status hello-world

# Upgrade a release
helm upgrade hello-world ./hello-world-python-chart

# Uninstall a release
helm uninstall hello-world

# Dry run (preview without installing)
helm install hello-world ./hello-world-python-chart --dry-run --debug

# Render templates without installing
helm template hello-world ./hello-world-python-chart
```

---

## Troubleshooting

### Common Issues

**1. Image not found in minikube**
```bash
minikube image load hello-world-python:1.0.0
```

**2. Service not found**
- Make sure you've run `helm install` command
- Check if pods are running: `kubectl get pods`

**3. YAML parse errors**
- Verify proper indentation in YAML files
- Ensure there's a space after colons (`:`)
- Use `helm template` to validate templates

**4. Pod not starting**
```bash
# Check pod logs
kubectl logs <pod-name>

# Describe pod for events
kubectl describe pod <pod-name>
```

---

## Cleanup

To remove all resources:

```bash
# Uninstall the Helm release
helm uninstall hello-world

# Stop minikube (if using)
minikube stop
```

---

## Additional Resources

- [Helm Documentation](https://helm.sh/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
- [Flask Documentation](https://flask.palletsprojects.com/)

---

## License

This project is for educational purposes as part of the Plataformas 2 course.

---

##Photos
<img width="1019" height="436" alt="image" src="https://github.com/user-attachments/assets/73fa414c-7187-46af-990e-47a5c0ef6d29" />
<img width="1230" height="492" alt="image" src="https://github.com/user-attachments/assets/4fbad479-9123-47ad-82c1-20381385a7ad" />
<img width="1600" height="173" alt="image" src="https://github.com/user-attachments/assets/f6e9da72-e86b-4097-a99b-1128c2162107" />
<img width="1600" height="529" alt="image" src="https://github.com/user-attachments/assets/277517d3-7ffd-4cb6-b1c3-4f0a08cc92bb" />
<img width="1600" height="165" alt="image" src="https://github.com/user-attachments/assets/9799e5af-d09f-475e-9f2f-310bb1260d95" />
<img width="1600" height="72" alt="image" src="https://github.com/user-attachments/assets/b4b6d24a-ce8f-4c31-b3e6-3305dc0f7192" />
<img width="1290" height="574" alt="image" src="https://github.com/user-attachments/assets/dcff6e4b-4caa-4e06-88a4-799ba55bac97" />
<img width="1600" height="220" alt="image" src="https://github.com/user-attachments/assets/32af06b6-436b-4d59-80df-c4d42f97dad9" />
<img width="1600" height="175" alt="image" src="https://github.com/user-attachments/assets/192b47d9-b309-4396-a920-886ec519f36c" />
<img width="1600" height="377" alt="image" src="https://github.com/user-attachments/assets/502a8d9d-5159-4e4d-8354-f5195ed8a221" />
<img width="1600" height="474" alt="image" src="https://github.com/user-attachments/assets/97f2cb4a-3325-4b43-a1e0-67397359daf5" />



**Author:** Sergio Fernando Florez Sanabria  
**Student ID:** A00396046  
**Date:** November 2025
