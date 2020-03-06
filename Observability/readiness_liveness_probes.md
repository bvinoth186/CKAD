# Readiness & Liveness Probes
##### Readiness - Check if application within container is up (such as, health check), then route the traffic
##### liveness - to restart failed container

### Set up readiness for http
<details><summary>show</summary>

```bash
spec:
  containers:
    - name: nginx-container
      image: nginx
      readinessProbe:
        httpGet:
          path: /authn/health
          port: 3000
        initialDelaySeconds: 10
        periodSeconds: 10
        timeoutSeconds: 5
        failureThreshold: 8
```
</details>

### Set up readiness for db socket
<details><summary>show</summary>

```bash
spec:
  containers:
    - name: nginx-container
      image: nginx
      readinessProbe:
        tcpSocket:
          port: 3306
        initialDelaySeconds: 10
        periodSeconds: 10
        timeoutSeconds: 5
        failureThreshold: 8
```
</details>

### Set up readiness for db command
<details><summary>show</summary>

```bash
spec:
  containers:
    - name: nginx-container
      image: nginx
      readinessProbe:
        exec:
          command:
            - cat
            - /app/is_healthy
        initialDelaySeconds: 10
        periodSeconds: 10
        timeoutSeconds: 5
        failureThreshold: 8
```
</details>

### Do same for liveness 
<details><summary>show</summary>

```bash
Just change readiness to liveness
```
</details>

