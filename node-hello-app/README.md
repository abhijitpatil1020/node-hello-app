Build Docker Image:
 docker build -t node-hello:v1 .

Start minikube:
 minikube start --cpus=2 --memory=4096 --driver=docker

Enable ingress controller: 
minikube addons enable ingress

helm install node-hello ./charts/node-hello
Verify:
kubectl get pods
kubectl get svc
kubectl get ingress

Add entry to /etc/hosts (replace IP with Minikube IP):
192.168.49.2 hello.local

Test:
curl http://hello.local

Update app:
docker build -t node-hello:v2 .
helm upgrade node-hello ./charts/node-hello --set image.tag=v2

Rollback:
helm rollback node-hello 1

Remove Helm Release:
helm uninstall node-hello

Delete Minikube Cluster:
minikube delete --all


Production Considerations:

Push images to DockerHub/ECR instead of using local images.
Configure Horizontal Pod Autoscaler (HPA).
Monitoring: Integrate Prometheus + Grafana.
Resource Management: Define proper CPU/memory requests & limits.
