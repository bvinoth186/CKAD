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



### Create a configMap 'cmvolume' with values 'var8=val8', 'var9=val9'. Load this as a volume inside an nginx pod on path '/etc/lala'. Create the pod and 'ls' into the '/etc/lala' directory.
<details><summary>show</summary>

```bash
kubectl create configmap cmvolume --from-literal=var8=val8 --from-literal=var9=val9
kubectl run nginx --image=nginx --restart=Never -o yaml --dry-run > pod.yaml
vi pod.yaml
```

```bash
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    run: nginx
  name: nginx
spec:
  volumes: # add a volumes list
  - name: myvolume # just a name, you'll reference this in the pods
    configMap:
      name: cmvolume # name of your configmap
  containers:
  - image: nginx
    imagePullPolicy: IfNotPresent
    name: nginx
    resources: {}
    volumeMounts: # your volume mounts are listed here
    - name: myvolume # the name that you specified in pod.spec.volumes.name
      mountPath: /etc/lala # the path inside your container
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
</details>
