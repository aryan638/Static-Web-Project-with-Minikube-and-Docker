### Steps to Run Your Static Web Project on Minikube:
### 1. **Install Minikube & Docker (if not already installed)**

- **Install Minikube**: [Minikube Installation](https://minikube.sigs.k8s.io/docs/)
- **Install Docker**: [Docker Installation](https://docs.docker.com/get-docker/)
### 2. **Start Minikube Cluster**
Start Minikube using the default driver (Docker in your case):

```bash
minikube start --driver=docker
```
### 3. **Set Up Docker Environment**
Tell Docker to use Minikube's Docker daemon to build the image:

```bash
eval $(minikube -p minikube docker-env)
```

This command needs to be run in **Linux/macOS**; on **Windows**, use:

```powershell
& minikube -p minikube docker-env | Invoke-Expression
```
### 4. **Build Docker Image**
Build your Docker image for your static web project:

```bash
docker build -t static-web-project .
```
This assumes you have a `Dockerfile` in your project directory.

### 5. **Create Deployment and Service YAML Files**
Create the Kubernetes deployment (`deployment.yaml`) and service (`service.yaml`) files to deploy your application on Minikube.

**Example `deployment.yaml`:**
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: static-web-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: static-web
  template:
    metadata:
      labels:
        app: static-web
    spec:
      containers:
      - name: static-web-container
        image: static-web-project  # Your Docker image name
        ports:
        - containerPort: 80
```
**Example `service.yaml`:**
```yaml
apiVersion: v1
kind: Service
metadata:
  name: static-web-service
spec:
  selector:
    app: static-web
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: NodePort
```
### 6. **Deploy the Application on Kubernetes**
Apply the `deployment.yaml` and `service.yaml` files to deploy your app on Minikube:

```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```
### 7. **Verify Pod and Service Status**
Check that your pods and services are running:
```bash
kubectl get pods
kubectl get services
```
Make sure the pod status is `Running` and the service has a `NodePort` assigned.
### 8. **Access the Application**
Get the Minikube IP address:

```bash
minikube ip
```
Use the IP and the port assigned by the `NodePort` to access your app in the browser:
```
http://<minikube-ip>:<node-port>
```
### 9. **(Optional) Port Forwarding for Testing**
If you're having issues accessing through the Minikube IP, use `kubectl port-forward` to forward a port locally:

```bash
kubectl port-forward pod/<pod-name> 8080:80
```
Then, access your app via `http://localhost:8080`.
### 10. **Verify the Web Application**
Now, you should be able to access your static web application from the browser!
---
### Troubleshooting:
- **ImagePullBackOff**: Ensure that your Docker image is built in the correct Minikube environment.
- **Port Issues**: If the port is already in use, either kill the process using it or change the local port in the `port-forward` command.

