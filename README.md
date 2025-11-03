# 🚀 React App Deployment on Kubernetes using Helm

This project demonstrates a complete DevOps workflow for deploying a **React.js web application** on a **Kubernetes cluster** using **Docker** and **Helm**.  
It shows how to containerize the app, push the image to Docker Hub, create Helm charts, and deploy the app seamlessly on a K8s cluster.

---

## 🧰 Tech Stack
- **Frontend:** React.js  
- **Containerization:** Docker  
- **Orchestration:** Kubernetes (K8s)  
- **Package Manager:** Helm  
- **Cloud Platform:** AWS EC2 (1 master node, 2 worker nodes)

---

## 🎯 Project Flow Summary

| Step | Description | Tools |
|------|--------------|--------|
| **1️⃣ React App Creation** | Create a simple frontend using Create React App | Node.js |
| **2️⃣ Dockerize the App** | Build a Docker image for the React app | Docker |
| **3️⃣ Push Image to Registry** | Push the image to Docker Hub | Docker Hub |
| **4️⃣ Setup Kubernetes Cluster** | Use master & worker nodes on AWS EC2 | kubeadm / kubectl |
| **5️⃣ Install Helm on Master Node** | Install Helm to manage K8s deployments | Helm |
| **6️⃣ Create Helm Chart** | Define deployment and service YAMLs | Helm |
| **7️⃣ Deploy with Helm** | Deploy the Dockerized app to Kubernetes | Helm install |
| **8️⃣ Access Application** | Use NodePort service and worker node IP | Browser |

---

## ⚙️ Prerequisites

Before starting, ensure you have:
- 🐋 Docker installed on one EC2 instance (for building the image)
- ☸️ Kubernetes cluster ready (1 master, 2 workers)
- 🔧 Helm installed on the master node
- 🌐 Docker Hub account for pushing images
- ⚙️ kubectl configured to access your cluster

---

## 🧩 Step-by-Step Implementation

### **Step 1: Create React Application**
```bash
npx create-react-app project1
cd project1


Step 2: Create Dockerfile

Create a file named Dockerfile inside your React project folder:

# Build Stage
FROM node:18-alpine AS build
WORKDIR /app
COPY . .
RUN npm install && npm run build

# Production Stage
FROM nginx:alpine
COPY --from=build /app/build /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]


Build and push the image:

docker build -t <your-dockerhub-username>/project1:latest .
docker login
docker push <your-dockerhub-username>/project1:latest

Step 3: Setup Kubernetes Cluster

Launch 3 EC2 instances (1 master + 2 workers)

Install Docker, kubeadm, kubelet, and kubectl

Initialize the master node:

sudo kubeadm init


Join worker nodes using the join command output

Verify nodes:

kubectl get nodes

Step 4: Install Helm on Master Node
curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
helm version

Step 5: Create Helm Chart

Generate a new chart:

helm create react-chart


Now simplify it for a React app by keeping only:

templates/deployment.yaml

templates/service.yaml

values.yaml

Delete the rest (tests, _helpers.tpl, ingress, etc.)

Step 6: Define Helm Files
📁 values.yaml
replicaCount: 1

image:
  repository: <your-dockerhub-username>/project1
  tag: "latest"
  pullPolicy: IfNotPresent

service:
  type: NodePort
  port: 80

resources: {}

nodeSelector: {}
tolerations: []
affinity: {}

📄 deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: react-deployment
  labels:
    app: react-app
spec:
  replicas: {{ .Values.replicaCount }}
  selector:
    matchLabels:
      app: react-app
  template:
    metadata:
      labels:
        app: react-app
    spec:
      containers:
        - name: react-container
          image: "{{ .Values.image.repository }}:{{ .Values.image.tag }}"
          imagePullPolicy: {{ .Values.image.pullPolicy }}
          ports:
            - containerPort: 80

📄 service.yaml
apiVersion: v1
kind: Service
metadata:
  name: react-service
spec:
  type: {{ .Values.service.type }}
  selector:
    app: react-app
  ports:
    - port: {{ .Values.service.port }}
      targetPort: 80
      protocol: TCP

Step 7: Deploy Application using Helm

Run:

helm install project1 ./react-chart


Verify:

kubectl get pods
kubectl get svc


Example output:

NAME             TYPE        CLUSTER-IP     EXTERNAL-IP   PORT(S)        AGE
react-service    NodePort    10.96.120.45   <none>        80:30567/TCP   5m

Step 8: Access the React App

Find worker node IPs:

kubectl get nodes -o wide


Copy the EXTERNAL-IP of any worker node

Access your app:

http://<worker-node-external-ip>:<nodeport>


Example:

http://3.87.99.200:30567

Step 9: Update or Upgrade App

After updating the React app and pushing a new image:

helm upgrade project1 ./react-chart

Step 10: Uninstall Application

To remove all resources:

helm uninstall project1