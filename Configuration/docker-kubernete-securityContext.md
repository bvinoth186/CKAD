# Docker-security

### Give docker container more permission
<details><summary>show</summary>

```bash
Capabilities are located at : /usr/include/linux/capabilities.h
docker run --cap-add MAC_ADMIN ubunty
```
</details>

### Remove docker container permission
<details><summary>show</summary>

```bash
docker run --cap-drop MAC_ADMIN ubunty
```
</details>

### Give docker container full priviledge
<details><summary>show</summary>

```bash
docker run --priviledged ubunty
```
</details>

### Run docker container as different user
<details><summary>show</summary>

```bash
You can add it in dockerFile as `USER 1000`
docker run --user=1000 ubuntu sleep 3600
```
</details>

### Find which is the current user in the pod
<details><summary>show</summary>

```bash
kubectl exec ubuntu-sleeper whoami
```
</details>

### Set securityContext at Pod level
<details><summary>show</summary>

```bash
apiVersion: v1
kind: Pod
metadata:
  name: app-pod
  labels:
    type: webserver
spec:
  securityContext:
    runAsUser: 1000
  containers:
    - name: ubuntu-container
      image: ubuntu
      command: ["sleep","3600"]
```
</details>


### Set securityContext at Container level
<details><summary>show</summary>
```bash
Capabilities are supported at only Container level NOT Pod Level

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
      securityContext:
        runAsUser: 1000
        capabilities:   //Capabilities are supported at only Container level NOT Pod Level
          add: ["MAC_ADMIN"]
```
</details>
