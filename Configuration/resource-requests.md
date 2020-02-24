# resource requests

### Request more memory/cpu for container/pod
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
      resources:
        requests:
          memory: "1Gi"
          cpu: 1
        limits:
          memory: "2Gi"
          cpu: 2    
  ```
</details>

### Set limit of container/pod for memory/cpu 
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
      resources:
        requests:
          memory: "1Gi"
          cpu: 1
        limits:
          memory: "2Gi"  // node can still give more than limit memory but if it is constant then pod will be terminated
          cpu: 2    // can't give more than this limit
  ```
</details>

### Set default requests/limit of container/pod for memory (create a LimitRange)
<details><summary>show</summary>

```bash
apiVersion: v1
kind: LimitRange
metadata:
  name: mem-limit-range
spec:
  limits:
  - default:
      memory: 512Mi
    defaultRequest:
      memory: 256Mi
    type: Container
  ```
</details>

### Set default requests/limit of container/pod for cpu  (create a LimitRange)
<details><summary>show</summary>

```bash
apiVersion: v1
kind: LimitRange
metadata:
  name: cpu-limit-range
spec:
  limits:
  - default:
      cpu: 1
    defaultRequest:
      cpu: 0.5
    type: Container
  ```
</details>