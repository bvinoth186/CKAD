# Secret

### Create secret [Imperative]
<details><summary>show</summary>

```bash
kubectl create  <secret-name> generic --from-literal=<key>=<value>
                              --from-literal=<key2>=<value2>

kubectl create secret generic app-secret --from-literal=DB_HOST=sql01

kubectl create  <secret-name> generic --from-file=app_secret.properties

kubectl create secret generic app-secret --from-file=app_secret.properties
```
</details>

### Create secret [Declarative]
<details><summary>show</summary>

```bash
--- secret.yaml ---
apiVersion: v1
kind: Secret
metadata:
  name: app-secret
data:
  DB_HOST: c3FsMDE=  // echo -n sql01 | base64 
  DB_USER: cm9vdA==
--- end ---
  
kubectl create -f secret.yaml
```
</details>

### Get secret info
<details><summary>show</summary>

```bash
kubectl get secrets
```
</details>

### Detail info about secrets
<details><summary>show</summary>

```bash
kubectl describe secrets
```
</details>

### Create a Pod with secret
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
      ports:
        - containerPort: 8080
      envFrom:
        - secretRef:
            name: app-secret
```

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
      ports:
        - containerPort: 8080
      env:
        - name: DB_HOST
          valueFrom:
            secretKeyRef:
              name: app-secret
              key: DB_HOST
```
</details>
