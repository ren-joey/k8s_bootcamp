## Environment
- Kind (Kubernetes in Docker)
- Kubectl
- Docker
- Kompose

## Installations
### Kind
[Official Guides](https://kind.sigs.k8s.io/)

You can download bin files through [kind github](https://github.com/kubernetes-sigs/kind/releases). Then add the `kind.exe` file into your system `PATH`. Once you have finished the configuration, you may check the installation by:
```bash
kind --version
```

### Kubectl
[Official Guides](https://kubernetes.io/zh-cn/docs/tasks/tools/#kubectl)

Same as the previous installation, you may check the installation by:
```bash
kubectl version --client
```

### Kompose
Kompose is a tool which can help you convert your `docker-compose.yml` file into multiple `yaml` files. You can use it through docker image method:
```bash
cd <path-to-your-docker-compose-file>

# build a kompose image
docker build -t kompose https://github.com/kubernetes/kompose.git
# Run a converting
# linux
docker run --rm -it -v $PWD:/opt kompose sh -c "cd /opt && kompose convert"
# windows
docker run --rm -it -v ${PWD}:/opt kompose sh -c "cd /opt && kompose convert"
```

## Start a cluster
```bash
# Create a new cluster
kind create cluster

# Check if the cluster created
kubectl get nodes

# Apply multiple images from docker (you should build each of them)
kubectl.exe apply -f ./

# Delete items from kubectl
kubectl delete all --all
kubectl delete configmap --all
kubectl delete secret --all
kubectl delete ingress --all
kubectl delete pvc --all

# Check all services
kubectl get svc

# Forward the traffic to external network
kubectl port-forward svc/my-service 80:80

# Check error logs
kubectl logs <pod-name>

# Check configurations from specific pod
kubectl describe svc <pod-name>
kubectl describe pod <pod-name>

# Check events
kubectl get events --sort-by=.metadata.creationTimestamp
```

## TODO
- Some containers could not run successfully
```bash
kubectl describe pod nestjs-web-5c8b955788-q5zt7
```
```
Events:
  Type     Reason     Age                   From               Message
  ----     ------     ----                  ----               -------
  Normal   Scheduled  15m                   default-scheduler  Successfully assigned default/nestjs-web-5c8b955788-q5zt7 to kind-control-plane
  Normal   Pulling    15m                   kubelet            Pulling image "node:20"
  Normal   Pulled     13m                   kubelet            Successfully pulled image "node:20" in 29.23s (1m6.926s including waiting). Image size: 398758453 bytes.
  Normal   Created    12m (x5 over 13m)     kubelet            Created container nestjs
  Normal   Started    12m (x5 over 13m)     kubelet            Started container nestjs
  Normal   Pulled     12m (x4 over 13m)     kubelet            Container image "node:20" already present on machine
  Warning  BackOff    4m48s (x43 over 13m)  kubelet            Back-off restarting failed container nestjs in pod nestjs-web-5c8b955788-q5zt7_default(d7b5ba38-4d11-4d19-96c2-2d24e319a44e)
```
- Maybe I should create `Dockerfile` for each service in `docker-compose.yml`
- Read [official guides](https://kind.sigs.k8s.io/) to learn Kind.