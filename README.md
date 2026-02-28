# sample-webapp
Jenkins
   ↓
Build Docker Image
   ↓
Push to DockerHub (ksiva2537/sample-webapp:tag)
   ↓
kubectl apply deployment.yaml
   ↓
Kubernetes pulls image from DockerHub
