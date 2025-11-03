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
