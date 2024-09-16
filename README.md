# Part 1 - Kubernetes workloads Challenge

#### Apply the file to create deployment       
`kubectl apply -f backend-deployment.yaml`

#### Check the deployment and pod status       

```
kubectl get deployment backend -n project-plato
kubectl get pods -n project-plato
```

#### check the /tmp file is writable and remaining filesystem are read-only        
`kubectl exec -it <pod-name(backend)> -n project-plato -- /bin/bash`

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
`kubectl get networkpolicy -n project-plato`

#### Testing Network policy functionality      
```
kubectl exec -it <pod-name(backend)> -n project-plato -- /bin/bash
telnet db1-service 6379
telnet db2-service 5432
```
If we try to telnet any other services or ports it will fail as we have limited the communication with Network policy.
