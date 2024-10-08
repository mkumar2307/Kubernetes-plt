# Part 1 - Kubernetes workloads Challenge

#### create name space for project and verify       
```
kubectl create namespace project-plato
kubectl get namespaces
```

#### Apply the file to create deployment       
`kubectl apply -f backend-deployment.yaml`

#### Check the deployment and pod status       

```
kubectl get deployment backend -n project-plato
kubectl get pods -n project-plato
```

#### check the /tmp file is writable and remaining filesystem are read-only        
`kubectl exec -it <pod-name(backend)> -n project-plato -- sh`

#### After exec try to write in /tmp      

```
touch /tmp/test-file
echo "write success" > /tmp/test-file
cat /tmp/test-file
```

#### check other filesystems for read-only        
`touch /root/test-file`

#### Create db1 and db2 deployments        

```
kubectl apply -f db1-deployment.yaml
kubectl apply -f db2-deployment.yaml
```

#### create services for db1 and db2 to expose requested ports        

```
kubectl apply -f db1-service.yaml
kubectl apply -f db2-service.yaml
```
#### verify created services      

`kubectl get svc -n project-plato`

#### Redeploy the updated backend deployment file      
`kubectl apply -f backend-deployment.yaml`

#### Check pod readiness status        
`kubectl get pods -n project-plato`

#### Scale deployment db1 to 0 replicas to check readiness failure and check readiness status again      
`kubectl scale deployment db1 --replicas=0 -n project-plato`

#### Scale back deployment db1 to 1 and verify the status       
`kubectl scale deployment db1 --replicas=1 -n project-plato`

#### Apply the network policy       
`kubectl apply -f np-backend.yaml`

#### check the network policy running or not        
`kubectl get networkpolicies -n project-plato`

![Diagram](images/Screenshot3.jpeg)

#### Testing Network policy functionality      
```
kubectl exec -it <pod-name(backend)> -n project-plato -- sh
telnet db1-service 6379
telnet db2-service 5432
```
![Diagram](images/Screenshot2.jpeg)
If we try to telnet any other services or ports it will fail as we have limited the communication with Network policy.

#### Create a secret for db2       
`kubectl apply -f db2-secret.yaml`

Update deployment db2 with secrets and redeploy.     

#### check the environment variables inside the db2 conatiner for verification     
```
kubectl exec -it <pod-name(db2)> -n project-plato -- sh
env | grep DB_
```
![Diagram](images/Screenshot1.jpeg)

#### Adding required repo and deploy ‘Postgres’ and 'kube-prometheus-stack' using its respective helm chart
```
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install postgres bitnami/postgresql --namespace project-plato
helm install prometheus-stack prometheus-community/kube-prometheus-stack --namespace project-plato
```
