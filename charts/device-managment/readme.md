## Docker
1. docker build -t vistzz/device-management:latest .
2. docker push vistzz/device-management:latest  
3. docker run --rm  -p 1234:8080 -e ASPNETCORE_HTTP_PORTS=8080 vistzz/device-management:latest


## Pods


kubectl get nodes
kubectl get pods
Создать ПОД
1. kubectl run device-managment --image=vistzz/device-managment --port=80 -e ASPNETCORE_HTTP_PORTS=8080
2. kubectl delete pods device-managment
3. kubectl run device-managment --image=vistzz/device-management:latest --port=80
4. kubectl describe pods
5. kubectl describe pods device-managment
6. kubectl port-forward device-managment 7780:80


apiVersion: v1
kind: Pod
metadata:
    name: device-managment
spec:
    containers:
      - name: container-apache
        image: vistzz/device-management:latest
        ports:
          - containerPort: 80

7. kubectl apply -f .\pod-ver1.yaml
8. kubectl delete -f .\pod-ver1.yamlhelm

docker run --rm  -p 80:8080 -e ASPNETCORE_HTTP_PORTS=8080 vistzz/device-management:latest

## Deployments

1. kubectl create deployment device-managment-deploy --image device-managment --image=vistzz/device-management:latest --port=80
2. kubectl get deploy
3. kubectl describe deployments 
4. kubectl scale

## Service
1. kubectl expose deployment device-managment-deployment --type=ClusterIP --port 80
2. kubectl get services
3. kubectl delete service deployment
4. minikube service device-managment-service --url

## Helm
0. helm create test-app
1. helm list
2. helm upgrade device-managment device-managment

## Ingress
1. helm upgrade --install ingress-nginx ingress-nginx  --repo https://kubernetes.github.io/ingress-nginx  --namespace ingress-nginx --create-namespace
2. minikube addons enable ingress
3. minikube tunnel
4. http://localhost/swagger/index.html

## Kafka helm
1. helm repo add bitnami https://charts.bitnami.com/bitnami
2. helm install my-kafka bitnami/kafka
3. helm dependency update

helm upgrade device-managment .\device-managment\ -f .\device-managment\values.yaml

## PVC
kubectl delete pvc data-device-managment-postgresql-0

## postgress
psql -h localhost -U postgres -tc "SELECT 1 FROM pg_roles WHERE rolname='myuser'" | grep -q 1 || psql -h localhost -U postgres -c "CREATE USER myuser WITH PASSWORD 'mypassword';";