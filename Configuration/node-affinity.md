### Node Affinity --> telling pod to assign to particular node
### Advanced node selector

### First Label the node
<details><summary>show</summary>

```bash
kubectl label nodes <node name> <label-key>=<label-value>

kubectl label nodes node-01 size=Large
```
</details>

### put pod to specific node using affinity
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
    - name: ubuntu-container
      image: ubuntu
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In  // can be NotIn,Exists
            values:
            - Large          

```
</details>
