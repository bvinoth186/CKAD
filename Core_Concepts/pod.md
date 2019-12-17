# Pods

### Run nginx pod with image nginx
<details><summary>show</summary>

```bash
kubectl run nginx --image nginx
```
</details>

### Get pods info
<details><summary>show</summary>

```bash
kubectl get pods
```
</details>

### Detail info about nginx pod
<details><summary>show</summary>

```bash
kubectl describe pod nginx
```
</details>

### Create pod-definitions.yml and create a pod nginx
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    type: webserver
spec:
  containers:
    - name: nginx
      image: nginx
```
```bash
kubectl create -f pod-def.yml
kubectl get pods [ kubectl get pod nginx-pod ]
```
</details>
