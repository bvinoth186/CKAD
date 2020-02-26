# Node Selector --> telling pod to assign to particular node
# has limitation - only 1 label...can't put or condition or "Not <>" conditions

### First Label the node
<details><summary>show</summary>

```bash
kubectl label nodes <node name> <label-key>=<label-value>

kubectl label nodes node-01 size=Large
```
</details>

### put pod to specific node
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
  nodeSelector:
    size: Large

```
</details>
