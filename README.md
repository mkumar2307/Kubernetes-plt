This is K8s repo

#Apply the file to create deployment
kubectl apply -f backend-deployment.yaml

#Check the deployment and pod status
kubectl get deployment backend -n project-plato
kubectl get pods -n project-plato

#check the /tmp file is writable and remaining filesystem are read-only
kubectl exec -it <pod-name> -n project-plato -- sh

#after exec try to write in /tmp
touch /tmp/test-file
echo "write success" > /tmp/test-file
cat /tmp/test-file

#check other filesystems for read-only
touch /root/test-file