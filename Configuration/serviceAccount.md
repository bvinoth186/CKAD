# ServiceAccount

### Create serviceAccount 
<details><summary>show</summary>

```bash
kubectl create  serviceaccount dashboard-sa
```
</details>

### Get Token of service Account
<details><summary>show</summary>

```bash
kubectl describe  serviceaccount dashboard-sa

Get the <token-name> from Token field

kubectl get secret <token-name>

Also to view token, you can
kubectl exec -it my-kubernetes-dashboard cat /var/run/secrets/kubernetes.io/serviceaccount/token
```
</details>


### Create pod-definitions.yml with serviceaccount
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
  serviceAccount: dashboard-sa
```
</details>

### By default, default serviceAccount is mounted. To disable that,
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
  automountServiceAccountToken: false
```
</details>
