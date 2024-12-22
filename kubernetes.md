## Create a Cluster

```shell
minikube start
```

## Deploy an App

"A Pod is a Kubernetes abstraction that represents a group of one or more application containers (such as Docker), and some shared resources for those containers."

```shell
kubectl create deployment kubernetes-bootcamp --image=gcr.io/google-samples/kubernetes-bootcamp:v1
```

## Explore your App

```shell
# im 2. terminal
kubectl proxy

# main terminal
export POD_NAME="$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')"
echo $POD_NAME
curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME:8080/proxy/
```

## Expose Your App Publicly
### Creating a service
To expose a kubernetes app to the outside world we have to use
services.

```shell
kubectl expose deployment/kubernetes-bootcamp --type="NodePort" --port 8080

export NODE_PORT="$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')"
echo "NODE_PORT=$NODE_PORT"

# If you are on Docker Desktop, you have to run a proxy, because docker desktop containers are isolated from the host.
# In a seperate Window run:
# minikube service kubernetes-bootcamp --url
# then examine the output, it should look like and url like: curl 127.0.0.1:51082
# curl 127.0.0.1:51082

curl http://"$(minikube ip):$NODE_PORT" 
```
### Using labels
Deployment automatically generated a label for our pod.
When executing the describe deployment command, you will see the Name (or key) of that label.
```shell
kubectl describe deployment 
```
We can use this label qeury a list of pods or list the existing Services to affiliated with that deployment.
```shell
kubectl get pods -l app=kubernetes-bootcamp
kubectl get services -l app=kubernetes-bootcamp
```

To add a new label to a pod you do:
```shell
kubectl label pods $POD_NAME version=v1
kubectl get pods -l version=v1
```

### Deleting a service

To delete a service, you can use labels as well.

```
kubectl delete service -l app=kubernetes-bootcamp
```

## Scale Your App


```shell
# replace the service with a loadbalancer
kubectl delete service kubernetes-bootcamp
kubectl expose deployment/kubernetes-bootcamp --type="LoadBalancer" --port 8080

# scale up to 4
kubectl scale deployments/kubernetes-bootcamp --replicas 4

# test it
export NODE_PORT="$(kubectl get services/kubernetes-bootcamp -o go-template='{{(index .spec.ports 0).nodePort}}')"
echo NODE_PORT=$NODE_PORT
# exec multiple times:
curl http://"$(minikube ip):$NODE_PORT"
# it should be load balanced now
```

## Update Your App

Kubernets allows rolling updates, you can update your service with zero downtime.
It does incrementally replace the current pods with new ones.

This command replaces the deployment of the application to version 2.
```
kubectl set image deployments/kubernetes-bootcamp kubernetes-bootcamp=docker.io/jocatalin/kubernetes-bootcamp:v2
```

Check the status of the rollout with this:
```
kubectl rollout status deployments/kubernetes-bootcamp
kubectl describe pods
```

To rollback the deployment to the last version:
```
kubectl rollout undo deployments/kubernetes-bootcamp
```
