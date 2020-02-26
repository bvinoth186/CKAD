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

####      requiredDuringSchedulingIgnoredDuringExecution:  pod must have affinity to put it on a node. If node is already in pod, ignore
####      preferredDuringSchedulingIgnoredDuringExecution: pod may/may not have affinity to put it on a node and can be placed to tainted node even no label assigned
####      requiredDuringSchedulingRequiredDuringExecution


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
      requiredDuringSchedulingIgnoredDuringExecution:  // preferredDuringSchedulingIgnoredDuringExecution, requiredDuringSchedulingRequiredDuringExecution
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In  // can be NotIn,Exists
            values:
            - Large          

```
</details>
