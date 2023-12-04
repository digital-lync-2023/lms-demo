# LMS-JAVA MINIKUBE DEPLOYMENT
## STEPS:
- Launch Server
- Install Software
- Create K8S Manifest files
- Deploy K8S files
## STEP-1: Launch Server
- Guide - https://minikube.sigs.k8s.io/docs/start/
- Requirements —-------t2.medium instance in AWS
- 2 CPUs or more
2GB of free ram memory
30GB of free disk space
## STEP-2: Install Softwares
### Update system
- sudo apt update
### Docker setup
Visit: https://get.docker.com/
curl -fsSL https://get.docker.com -o install-docker.sh
sudo sh install-docker.sh
sudo usermod -aG docker ubuntu
newgrp docker
docker -v
### Kubectl setup
Visit: https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#install-kubectl-binary-with-curl-on-linux
curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/kubectl
kubectl version
### Minikube setup
Visit: https://minikube.sigs.k8s.io/docs/start/
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
minikube version
## STEP-3: Create K8S Manifest files
Code: git clone -b minikube https://github.com/muralialakuntla3/lms-java.git
### Database:
#### mysql-secrets.yml
**echo -n Qwerty@123 | base64**
**UXdlcnR5QDEyMw==**
    apiVersion: v1
    kind: Secret
    metadata:
      name: mysql-secret
    data:
      password: UXdlcnR5QDEyMw==
#### mysql-deployment.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mysql-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mysql
  template:
    metadata:
      labels:
        app: mysql
    spec:
      containers:
      - name: mysql
        image: mysql:latest
        ports:
        - containerPort: 3306
        env:
        - name: MYSQL_ROOT_PASSWORD
          valueFrom:
            secretKeyRef:
              name: mysql-secret
              key: password
#### mysql-cluster-ip.yml
apiVersion: v1
kind: Service
metadata:
  name: mysql-cluster-ip
spec:
  selector:
    app: mysql
  ports:
    - protocol: TCP
      port: 3306
      targetPort: 3306
  type: ClusterIP

Backend:
Frontend:




STEP-4: Deploy K8S files


