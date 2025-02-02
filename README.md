### Repository for the K8s in 1 hour video
[Repository for the K8s in 1 hour video](https://www.youtube.com/watch?v=s_o8dwzRlu4)
#### K8s manifest files 
* mongo-config.yaml
* mongo-secret.yaml
* mongo.yaml
* webapp.yaml

#### K8s commands

##### start Minikube and check status
    minikube start --vm-driver=hyperkit 
    minikube status

##### get minikube node's ip address
    minikube ip

##### get basic info about k8s components
    kubectl get node
    kubectl get pod
    kubectl get svc
    kubectl get all

##### get extended info about components
    kubectl get pod -o wide
    kubectl get node -o wide

##### get detailed info about a specific component
    kubectl describe svc {svc-name}
    kubectl describe pod {pod-name}

##### get application logs
    kubectl logs {pod-name}
    
##### stop your Minikube cluster
    minikube stop

<br />

> :warning: **Known issue - Minikube IP not accessible** 

If you can't access the NodePort service webapp with `MinikubeIP:NodePort`, execute the following command:
    
    minikube service webapp-service

<br />

#### Expose the service with Ingress 

```shell
# enable ingress controller for minkube
minikube addons enable ingress
kubectl get pods -n ingress-nginx
kubectl get svc -n ingress-nginx


# apply the ingress 
kubectl apply -f ./webapp-ingress.yaml

kubectl get ingress webapp-ingress

# let Kubernetes ingress controller watch on 80 port
sudo minikube tunnel

# manually resolve the domain to 127.0.0.1
curl --resolve "webapp.example.com:80:127.0.0.1" -i http://webapp.example.com

# you can also persist the dns resovle record into /etc/hosts
127.0.0.1 webapp.example.com   
127.0.0.1 hello-world.example

```

```shell
kubectl create secret tls webapp-tls --cert=path/to/tls.crt --key=path/to/tls.key

```

### Reverse engineering 

#### Export a Pod to YAML

```shell
kubectl get pods

kubectl get pod <pod-name> -o yaml > pod.yaml
```

#### Export a Service to YAML

```shell
kubectl get services

kubectl get service <service-name> -o yaml > service.yaml
```

#### Export a Deployment to YAML

```shell
kubectl get deployments

kubectl get deployment <deployment-name> -o yaml > deployment.yaml
```

#### Links
* mongodb image on Docker Hub: https://hub.docker.com/_/mongo
* webapp image on Docker Hub: https://hub.docker.com/repository/docker/nanajanashia/k8s-demo-app
* k8s official documentation: https://kubernetes.io/docs/home/
* webapp code repo: https://gitlab.com/nanuchi/developing-with-docker/-/tree/feature/k8s-in-hour
* ingress on minikube https://kubernetes.io/docs/tasks/access-application-cluster/ingress-minikube/
