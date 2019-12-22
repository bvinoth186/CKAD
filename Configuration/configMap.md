# ConfigMap

### Create config map [Imperative]
<details><summary>show</summary>

```bash
kubectl create  <config-name> --from-literal=<key>=<value>
                              --from-literal=<key2>=<value2>

kubectl create configmap app-config --from-literal=APP_COLOR=blue

kubectl create  <config-name> --from-file=app_config.properties

kubectl create configmap app-config --from-file=app_config.properties
```
</details>

### Create config map [Declarative]
<details><summary>show</summary>

```bash
--- config-map.yaml ---
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_COLOR: blue
  APP_MODE: prod
--- end ---
  
kubectl create -f config-map.yaml
```
</details>

### Get configMaps info
<details><summary>show</summary>

```bash
kubectl get configmaps
```
</details>

### Detail info about configmaps
<details><summary>show</summary>

```bash
kubectl describe configmaps 
```
</details>

### Create a Pod with config
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
        - configMapRef:
            name: app-config 
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
        - name: APP_COLOR
          valueFrom:
            configMapKeyRef:
              name: app-config
              key: APP_COLOR
```
</details>
