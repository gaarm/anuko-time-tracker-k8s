# Instructions
Instructions how to setup `Anuko Time Tracker` in Kubernetes cluster.

## Configuration
### Docker engine 
Make sure Docker engine is installed and running on your machine. If not, please follow this instructions:
<a href="https://docs.docker.com/install/">Install Docker</a>

### Kubernetes
Make sure you have a running Kubernetes cluster. If not, you can install and run Kubernetes locally on your machine.
In that case follow this instructions: 
<a href="https://kubernetes.io/docs/setup/learning-environment/minikube/">How to install Minikube</a>.

## Create Docker images
Please download `Anuko Time Tracker` application.
```
wget -c https://www.anuko.com/download/time_tracker/tt_trial_docker.zip
unzip tt_trial_docker.zip
```

By executing bellow command docker images will get created on your machine:  
`docker load -i tt_trial_docker.tar`
 
You can check for existence of docker images by executing following command:  
`docker images`

> Docker images are referenced in deployment files. See deployment.yaml and deployment_db.yaml for more info.

## Run application in Kubernetes cluster
Run bellow commands:
```
kubectl apply -f pv_pvc.yaml
kubectl apply -f deployment_db.yaml
kubectl apply -f deployment.yaml
```

By doing so following Kubernetes objects will be created: 
`pods, services, deployments, persistent volume, persistence volume claim`

You can check which pods are running by executing following command:  
`kubectl get pods`

Output should look like this:
```
NAME                                    READY     STATUS              RESTARTS   AGE
anuko-timetracker-5b566c995d-s7gk9      1/1       Running             0          1m
anuko-timetracker-db-7f554c375d-g5hd3   1/1       Running             0          1m
```
### Exposing application
By executing bellow command, `Anuko Time Tracker` application will be available on `http://localhost:12345`
```
kubectl port-forward anuko-timetracker-5b566c995d-s7gk9  12345:80
```
```
Forwarding from 127.0.0.1:12345 -> 80
Forwarding from [::1]:12345 -> 80
Handling connection for 12345
```
> Please note that you have to replace pod name with the actual pod name running in your application.