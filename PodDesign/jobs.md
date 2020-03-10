## Jobs

### Create a job with image perl that runs the command with arguments "perl -Mbignum=bpi -wle 'print bpi(2000)'"

<details><summary>show</summary>
<p>

```bash
kubectl run pi --image=perl --restart=OnFailure -- perl -Mbignum=bpi -wle 'print bpi(2000)'
```

**However**, `kubectl run` for Job is Deprecated and will be removed in a future version. What you can do is:

```bash
kubectl create job pi  --image=perl -- perl -Mbignum=bpi -wle 'print bpi(2000)'
```

</p>
</details>

### Wait till it's done, get the output

<details><summary>show</summary>
<p>

```bash
kubectl get jobs -w # wait till 'SUCCESSFUL' is 1 (will take some time, perl image might be big)
kubectl get po # get the pod name
kubectl logs pi-**** # get the pi numbers
kubectl delete job pi
```

</p>
</details>

### Create a job with the image busybox that executes the command 'echo hello;sleep 30;echo world'

<details><summary>show</summary>
<p>

```bash
kubectl run busybox --image=busybox --restart=OnFailure -- /bin/sh -c 'echo hello;sleep 30;echo world'
```

**However**, `kubectl run` for Job is Deprecated and will be removed in a future version. What you can do is:

```bash
kubectl create job busybox --image=busybox -- /bin/sh -c 'echo hello;sleep 30;echo world'
```

</p>
</details>

### Follow the logs for the pod (you'll wait for 30 seconds)

<details><summary>show</summary>
<p>

```bash
kubectl get po # find the job pod
kubectl logs busybox-ptx58 -f # follow the logs
```

</p>
</details>

### See the status of the job, describe it and see the logs

<details><summary>show</summary>
<p>

```bash
kubectl get jobs
kubectl describe jobs busybox
kubectl logs job/busybox
```

</p>
</details>

### Delete the job

<details><summary>show</summary>
<p>

```bash
kubectl delete job busybox
```

</p>
</details>

### Create a job but ensure that it will be automatically terminated by kubernetes if it takes more than 30 seconds to execute

<details><summary>show</summary>
<p>
  
```bash
kubectl create job busybox --image=busybox --dry-run -o yaml -- /bin/sh -c 'while true; do echo hello; sleep 10;done' > job.yaml
vi job.yaml
```
  
Add job.spec.activeDeadlineSeconds=30

```bash
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  activeDeadlineSeconds: 30 # add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: busybox
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - while true; do echo hello; sleep 10;done
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: OnFailure
status: {}
```
</p>
</details>

### Create the same job, make it run 5 times, one after the other. Verify its status and delete it

<details><summary>show</summary>
<p>

```bash
kubectl create job busybox --image=busybox --dry-run -o yaml -- /bin/sh -c 'echo hello;sleep 30;echo world' > job.yaml
vi job.yaml
```

Add job.spec.completions=5

```YAML
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  completions: 5 # add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: busybox
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;echo world
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: OnFailure
status: {}
```

```bash
kubectl create -f job.yaml
```

Verify that it has been completed:

```bash
kubectl get job busybox -w # will take two and a half minutes
kubectl delete jobs busybox
```

</p>
</details>

### Create the same job, but make it run 5 parallel times

<details><summary>show</summary>
<p>

```bash
vi job.yaml
```

Add job.spec.parallelism=5

```YAML
apiVersion: batch/v1
kind: Job
metadata:
  creationTimestamp: null
  labels:
    run: busybox
  name: busybox
spec:
  parallelism: 5 # add this line
  template:
    metadata:
      creationTimestamp: null
      labels:
        run: busybox
    spec:
      containers:
      - args:
        - /bin/sh
        - -c
        - echo hello;sleep 30;echo world
        image: busybox
        name: busybox
        resources: {}
      restartPolicy: OnFailure
status: {}
```

```bash
kubectl create -f job.yaml
kubectl get jobs
```

It will take some time for the parallel jobs to finish (>= 30 seconds)

```bash
kubectl delete job busybox
```

</p>
</details>
