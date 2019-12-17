# Pods

### Run nginx pod with image nginx

<details><summary>show</summary>
<p>

```bash
kubectl run nginx --image nginx
```

</p>
</details>

### Get pods info

<details><summary>show</summary>
<p>

```bash
kubectl get pods
```

</p>
</details>

### Detail info about nginx pod

<details><summary>show</summary>
<p>

```bash
kubectl describe pod nginx
```

</p>
</details>

### Create pod-definitions.yml and create a pod nginx

<details><summary>show</summary>
<p>

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

</p>
</details>
