# Deployment

### Create deploy-definitions.yml 
<details><summary>show</summary>

```bash
apiVersion: apps/v1
kind: Deployment
metadata:
  name: myapp-deployment
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
kubectl create -f deploy-def.yml
kubectl get deployments [ kubectl get deployment myapp-deployment ]
```
</details>

### Create a deployment Imperative way  
<details><summary>show</summary>

```bash
// For deployment, you do not need generator
kubectl run nginx --image=nginx

Recommended way:
kubectl create deployment --image=nginx nginx
```
</details>

### Get Deployment info
<details><summary>show</summary>

```bash
kubectl get deployments
kubectl get deployments -o wide  // give which containers are in deployment
```
</details>

### Detail info about deployment
<details><summary>show</summary>

```bash
kubectl describe deployments myapp-deployment
```
</details>

### Detail info about everything
<details><summary>show</summary>

```bash
kubectl get all
```
</details>

### Delete using dep-def.yml 
<details><summary>show</summary>

```bash
kubectl delete -f dep-def.yml
```
</details>

### Delete using deployment name
<details><summary>show</summary>

```bash
kubectl delete deployments myapp-deployment
```
</details>

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run)
<details><summary>show</summary>

```bash
kubectl run nginx --image=nginx --dry-run -o yaml
OR
kubectl create deployment --image=nginx nginx --dry-run -o yaml
```
</details>

### Generate Deployment YAML file (-o yaml). Don't create it(--dry-run) with 4 Replicas (--replicas=4)
<details><summary>show</summary>

```bash
kubectl run nginx --image=nginx --dry-run --replicas=4 -o yaml

//kubectl create deployment does not have a --replicas option. You could first create it and then scale it using the kubectl scale command
```
</details>

### Generate Deployment YAML file (-o yaml). Create with 4 Replicas (--replicas=4)
<details><summary>show</summary>

```bash
kubectl run nginx --image=nginx --dry-run --replicas=4 -o yaml > nginx-deployment.yaml
```
</details>
