# Task 5: Build a Kubernetes Cluster Locally with Minikube

## 🎯 Objective
Deploy and manage apps in Kubernetes using Minikube and `kubectl`.

---

## 🛠 Tools Used
- Minikube
- Docker
- kubectl
- Windows 11 (with Docker Desktop)

---

## 📦 Step-by-Step Implementation
### ✅ 1. Install Minikube
✅ Step-by-Step Guide to Install Minikube via CMD
1. Open CMD as Administrator
Press Win + X → Choose Command Prompt (Admin) or search cmd, right-click, and choose Run as administrator.

2. Install Chocolatey (if not already installed)
Chocolatey is a package manager for Windows. If you don’t have it:

## - @"%SystemRoot%\System32\WindowsPowerShell\v1.0\powershell.exe" -NoProfile -InputFormat None -ExecutionPolicy Bypass -Command "Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))" && SET "PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin"

Close and reopen CMD after installation.

3. Install Minikube

- choco install minikube

- choco install kubernetes-cli

![1](https://github.com/user-attachments/assets/1630c954-7ac1-4f81-b7a5-5ef7eeaf6f35)

- minikube version

- kubectl version --client


![2](https://github.com/user-attachments/assets/684705df-c397-4c4d-a5e6-df3a88d16e7c)

### ✅ 2. Start Minikube

- minikube start --driver=docker

Make sure Docker Desktop is running before executing this.

## 📁 1. Create a pod YAMl
## mypod.yaml

apiVersion: v1
kind: Pod
metadata:
  name: portfolio
  labels:
    app: portfolio
spec:
  containers:
  - name: portfolio
    image: athithyan402/hosted-docker-website:latest
    ports:
    - containerPort: 80
   
## Apply the pod:

- kubectl apply -f mypod.yaml

## 📁 2. Create a Deployment YAML
## deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: portfolio-deployment
spec:
  replicas: 5
  selector:
    matchLabels:
      app: portfolio
  template:
    metadata:
      labels:
        app: portfolio
    spec:
      containers:
      - name: portfolio
        image: athithyan402/hosted-docker-website:latest
        ports:
        - containerPort: 80
## Apply the deployment:

- kubectl apply -f deployment.yaml
 
## 🌐 3. Create a Service YAML
## service.yaml

apiVersion: v1
kind: Service
metadata:
  name: portfolio-service
spec:
  type: NodePort
  selector:
    app: portfolio
  ports:
  - port: 80
    targetPort: 80
    
## Apply the service:

- kubectl apply -f service.yaml
  
## 🔍 4. Verify Pods and Services
## Check pods:

- kubectl get pods

![5](https://github.com/user-attachments/assets/2ad9683e-fb58-42cc-8f32-84b59c6eaee9)

## Check services:

- kubectl get svc

![6](https://github.com/user-attachments/assets/0c001c50-a576-47c2-836c-97af86d62da7)

## Check logs (for any pod):

- kubectl logs portfolio

![image](https://github.com/user-attachments/assets/d27d3ad6-d4d4-4b80-96a6-eed8fd883f83)

## Describe a pod for more details:

- kubectl describe pod portfolio

![image](https://github.com/user-attachments/assets/6828b498-2e11-4838-9d38-5ce14416e711)

## 📈 5. Scale the Deployment
## Scale up:

- kubectl scale deployment portfolio-deployment --replicas=10

Verify pods:

![image](https://github.com/user-attachments/assets/9748ce78-ab13-473a-b6e8-b9615fe33722)

## Scale down:

- kubectl scale deployment portfolio-deployment --replicas=5


Verify again:

- kubectl get pods

![image](https://github.com/user-attachments/assets/101ce9c8-c3fd-4979-998f-1b9bac95097e)


## 🌍 6. Access the App in Browser
## Get the Minikube IP:

- minikube ip

![image](https://github.com/user-attachments/assets/fe994f75-3522-47f9-a613-6a9dd10cc642)


Get the NodePort:

- kubectl get svc

![image](https://github.com/user-attachments/assets/cadbe1b8-6c94-450e-984e-8456dd3e0eb6)


## Open your browser and navigate to:

- http://192.168.49.2:31538

![10](https://github.com/user-attachments/assets/8fa4daeb-8194-41cb-afcc-71399f51a887)
