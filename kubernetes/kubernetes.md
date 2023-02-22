## what is kubernetes?
    system for running many diffrent containers over multiple diffrent machines

## why use kubernetes?
    when you need to run many different contaniers with different images 


## working with kubernetes

in development: minikube

in production: amazon elastic containers service for k8s(EKS) or google cloud k8s engine (GKE)



kubectl -> used for managing containers in the node

minikube -> used for managing the vm itself


note: minikube only used in local env but kubectl is used in both local & production

## start
```{r, engine='bash', count_lines}
kubectl apply -f <filename>
```
## info
```{r, engine='bash', count_lines}
kubectl get pods
```
```{r, engine='bash', count_lines}
kubectl get pods -o wide
```
```{r, engine='bash', count_lines}
kubectl get services
```
```{r, engine='bash', count_lines}
kubectl get deployments
```
```{r, engine='bash', count_lines}
kubectl get pv(persistent volume)
```

```{r, engine='bash', count_lines}
kubectl describe <object-type> <object-name>
ex: kubectl describe pod client-pod
```

## difference  between pod and deployment
pods :
    -> runs a single set of containers
    -> good for development purpose
    -> rarley used directly in production
Deployment:
    -> runs a set of identical pods(one or more)
    -> monitors the state of each pod, updating as necessary
    -> good for development and production







## delete pods/services/Deployments
```{r, engine='bash', count_lines}
kubectl delete -f <config file>
```
```{r, engine='bash', count_lines}
kubectl delete deployment <deployment-name>
```
```{r, engine='bash', count_lines}
kubectl delete service <service-name>
```

### if you updated docker image in docker hub how you will get a latest image in kubectl & change the currently running pod container without stoping it
```{r, engine='bash', count_lines}
kubectl set image <object-type>/<object-name> <container_name>=<new_image_to_use>
ex: kubectl set image deployment/client-deployment client=stephengrider/multi-client:v5

```

minikube is a vm , inside that vm you can see all the containers running on yours pods, we cant access them directly

```{r, engine='bash', count_lines}
docker exec -it minikube bash
docker ps
```

to access pods container directly in your pc without doing this, you need to reconfigure vm
```{r, engine='bash', count_lines}
eval $(minikube docker-env) -> this only configures your current terminal window
```
## go inside the container
```{r, engine='bash', count_lines}
kubectl exec -it <pod name>
```


## different object types


### pods -> runs one or more closely related containers
### services -> sets up networking in a kubernetes cluster
### clusterIp -> exposes a set of pods to other objects in the cluster
### nodePort -> exposes a set of pods to the outside world (only good for dev purposes)

clusterIp is goging to provide access for everything else inside of deployment it as attached to
if you define clusterIp for a deployment all other deployments can access this clusterIp, you cannot access clusterIp from outside world




### postgres db writes to file system

-> here is the thing to keep in mind about any file system that is created or maintained inside of a contaniner

-> any postgres container inside a pod if it get crashed we are going to lose all the data that exists inside the container, and kubernetes
  just going to start new pod as a replacement for that crashed pod so this new pod dont have data carried over from crashed pod to slove this 
  we use volume

-> volume will exist outside of the container
-> volume in k8s is an object that allows a container to store data at pod level
-> docker volume and k8s persistent volume are diffrent 

-> k8s volume and persistent volume are different
-> k8s volume will be attached to pod(if pod goes down we will loose the volume)
-> persistent volume will not get attached to any pod it is kind of like stored outside of the pod 
-> persistent volume claim and persistent volume are different


### create env variables
example of creating env variable inside the pod
```yaml
containers:
  - name: redis
    image: redis
    ports:
      - containerPort: 7000
    env:
      - name: REDIS_HOST
        value: redis-cluster-ip-service
      - name: REDIS_PORT
        value: 6379
```

### creating secret to store passwords
```{r, engine='bash', count_lines}
kubectl create secret generic <secret_name> --from-literal key=value
ex: kubectl create secret generic pgpassword --from-literal PGPASSWORD=123455
```

  referencing in pod

```yaml
  containers:
    -------
    -------
    - name: PGPASSWORD
      valueFrom: 
        secretKeyRef:
          name: pgpassword
          key: PGPASSWORD

```
using inside code -> process.env.PGPASSWORD

note: we cant give number inside secret it will be converted to string


## enabling ingress
```{r, engine='bash', count_lines}
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/130af33510882ae62c89277f2ad0baca50e0fafe/deploy/static/mandatory.yaml

minikube addons enable ingress
```

most common object types used 
  Deployment
  Service/ ClusterIP
  PersistentVolumeClaim
  Ingress


examples:



## Deployment , ClusterIP, PersistentVolumeClaim
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: postgres-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: postgres
  template:
    metadata:
      labels:
        component: postgres
    spec:
      volumes:
        - name: postgres-storage
          persistentVolumeClaim:
            claimName: database-persistent-volume-claim
      containers:
        - name: postgres
          image: postgres
          ports:
            - containerPort: 5432
          volumeMounts:
            - name: postgres-storage
              mountPath: /var/lib/postgresql/data
              subPath: postgres
--- 
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: database-persistent-volume-claim
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests: 
      storage: 2Gi
--- 
apiVersion: v1
kind: Service
metadata:
  name: postgres-cluster-ip-service
spec:
  type: ClusterIP
  selector:
    component: postgres
  ports:
    - port: 5432
      targetPort: 5432
```



## Ingress
```yaml
apiVersion: networking.k8s.io/v1beta1
kind: Ingress
metadata:
  name: ingress-service
  annotations:
    kubernetes.io/ingress.class: 'nginx'
    nginx.ingress.kubernetes.io/rewrite-target: /
spec:
  rules:
    - http:
        paths:
          - path: /
            backend:
              serviceName: client-cluster-ip-service
              servicePort: 3000
          - path: /api
            backend:
              serviceName: server-cluster-ip-service
              servicePort: 5000


```
## Deployment with env and secrets
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: redis-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      component: redis
  template:
    metadata:
      labels:
        component: redis
    spec:
      containers:
        - name: redis
          image: redis
          ports:
            - containerPort: 6379
          env:
            - name: REDIS_HOST
              value: redis-cluster-ip-service
            - name: REDIS_PORT
              value: 6379
        - name: PGPASSWORD
          valueFrom: 
            secretKeyRef:
              name: pgpassword
              key: PGPASSWORD
```