# ReplicaSet

### Create rs-definitions.yml 
<details><summary>show</summary>

```bash
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    type: webserver
spec:
  replicas: 3
  template:
    metadata:
      name: app-pod
      labels:
        type: webserver
    spec:
      containers:
        - name: nginx-container
          image: nginx
        - name: redis-container
          image: redis
  selector:
    matchLabels:
      type: webserver
```
```bash
kubectl create -f rs-def.yml
kubectl get rs [ kubectl get rs myapp-replicaset ]
```
</details>

### Get Replicaset info
<details><summary>show</summary>

```bash
kubectl get rs
kubectl get rs -o wide  // give which containers are in RS
```
</details>

### Detail info about Replicaset
<details><summary>show</summary>

```bash
kubectl describe rs myapp-replicaset
```
</details>

### Scale Replicaset
<details><summary>show</summary>

```bash
kubectl replace -f rs-def.yml
kubectl scale --replicas=6 -f rs-def.yml
kubectl scale --replicas=6 rs myapp-replicaset
```
</details>

### Delete using rs-def.yml 
<details><summary>show</summary>

```bash
kubectl delete -f rs-def.yml
```
</details>

### Delete using rs name
<details><summary>show</summary>

```bash
kubectl delete rs myapp-replicaset
```
</details>
