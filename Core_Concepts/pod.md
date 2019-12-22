# Pods

### Run nginx pod with image nginx
<details><summary>show</summary>

```bash
kubectl run nginx --image nginx

Imperative Command:
kubectl run --generator=run-pod/v1 nginx --image=nginx
```
</details>

### Get pods info
<details><summary>show</summary>

```bash
kubectl get pods
kubectl get pods -o wide  // Displays which node pod is located
```
</details>

### Detail info about nginx pod
<details><summary>show</summary>

```bash
kubectl describe pod nginx
```
</details>

### Create pod-definitions.yml and create a pod nginx and redis
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Pod
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
```
```bash
kubectl create -f pod-def.yml
kubectl get pods [ kubectl get pod app-pod ]
```
</details>

### create pod-definition-auto.yml from existing pod
<details><summary>show</summary>

```bash
kubectl get pod app-pod -o yaml > pod-definition-auto.yml
Then edit the file to make the necessary changes, delete and re-create the pod.
```
</details>

### edit yml file on the file without creating it
<details><summary>show</summary>

```bash
kubectl edit pod app-pod
Then edit the file to make the necessary changes, delete and re-create the pod.
```
</details>

### Delete using pod-definitions.yml 
<details><summary>show</summary>

```bash
kubectl delete -f pod-def.yml
```
</details>

### Delete using pod name
<details><summary>show</summary>

```bash
kubectl delete pod app-pod
```
</details>

### Generate POD Manifest YAML file (-o yaml). Don't create it(--dry-run)
<details><summary>show</summary>

```bash
kubectl run --generator=run-pod/v1 nginx --image=nginx --dry-run -o yaml
```
</details>


### Create pod-definitions.yml with commands & args & env
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Pod
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
      command: ["sleep"]
      args: ["2000"]
      env:
        - name: APP_COLOR
          value: pink
```
```bash
kubectl create -f pod-def.yml
kubectl get pods [ kubectl get pod app-pod ]
```
</details>


