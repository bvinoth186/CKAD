# Taint toleration --> telling nodes to accept certain pods only

### Taint a node (taint can be done at node and toleration at pod level)
<details><summary>show</summary>

```bash
kubectl taint nodes <node-name> key=value:taint-effect

## taint effect can have one of these values: NoSchedule| PreferNoSchedule| NoExecute
## taint effect - what happens to pod if Pod DO NOT TOLERATE this taint
## NoExecute - pod will be killed if pod is assigned to node and node is tainted afterwords

```
</details>

### Check if node has taint or not
<details><summary>show</summary>

```bash
kubectl describe node node01 | grep Taints

```
</details>

### Add tolerations to the pod
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
      command: ["sleep","3600"]
  tolerations:
    - key: "app"
      operator: "Equal"
      value: "blue"
      effect: NoSchedule
```
</details>
