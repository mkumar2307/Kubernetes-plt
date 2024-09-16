# Part 1 - Kubernetes workloads Challenge

#### Apply the file to create deployment       
`kubectl apply -f backend-deployment.yaml`

#### Check the deployment and pod status       

```
kubectl get deployment backend -n project-plato
kubectl get pods -n project-plato
```

#### check the /tmp file is writable and remaining filesystem are read-only        
`kubectl exec -it <pod-name> -n project-plato -- sh`

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