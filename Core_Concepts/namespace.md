# namespace

### Get namespaces info
<details><summary>show</summary>

```bash
kubectl get namespaces
```
</details>

### Create namespace-definitions.yml so that pod will be created in dev namespace
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
  namespace: dev
  labels:
    type: webserver
spec:
  containers:
    - name: nginx-container
      image: nginx
    - name: redis-container
      image: redis  
```
```bash
kubectl create -f pod-def.yml
kubectl get namespaces
kubectl get pods -n dev
```
</details>


### create namespace using file & by command
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Namespace
metadata:
  name: dev
```

```bash
kubectl create namespace dev
```
</details>


### Set current context to dev namespace
<details><summary>show</summary>

```bash
kubectl config set-context $(kubectl config current-context) --namespace=dev
```
</details>

### To view all pods from all namespaces
<details><summary>show</summary>

```bash
kubectl get pods --all-namespaces
```
</details>

### create resource quota for namespace
<details><summary>show</summary>

```bash
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
  namespace: dev
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```
</details>
