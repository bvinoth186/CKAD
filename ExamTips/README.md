# Bookmarks


# CKAD Exercises
- https://github.com/dgkanatsios/CKAD-exercises
- https://codeburst.io/kubernetes-ckad-weekly-challenges-overview-and-tips-7282b36a2681
- https://medium.com/@_matthewpalmer/smash-your-kubernetes-ckad-exam-my-study-guide-and-exam-resources-58326755e2c3


# CKAD Prep notes
- https://github.com/twajr/ckad-prep-notes


# CKAD Aliases
The following command sets all spacing (tab stop, soft tab stop, shift width) to two spaces, expands tabs to spaces, and turns on line numbering.
```
vi ~/.vimrc
set ts=2 sts=2 sw=2 et number

vim ~/.bashrc
# then add those two:
alias k='kubectl'
alias kn='kubectl config set-context --current --namespace ' // you can just do `kn np1` to switch to namespace. Mind the space

alias kcf='kubectl create -f'
alias kaf='kubectl apply -f'

alias kdf='k delete -f'

alias kgp='kubectl get pod'
alias kdgf='kubectl delete --grace-period=0 --force'

alias kdr= 'kubectl -o yaml --dry-run'
alias kexr='k explain --recursive'
alias krbb='kubectl run test --generator=run-pod/v1 -it --restart=Never --image=busybox --rm --'

# Autocompletion
source <(kubectl completion bash)

```

# CKAD Useful commands [imperative]
```
# Note: All examples have aliased kubectl to k --> alias k=kubectl

# Help about yaml file (make sure to use --recursive)
$ k explain Pod.spec.containers --recursive

# Create a NGINX pod
$ k run nginx --image=nginx --restart=Never

# Create a NGINX deployment with 3 replicas
$ k run nginx --image=nginx --replicas=3

# Create a Job based on the busybox image. Execute the command "sleep 4800"
$ k run bb-job --image=busybox --restart=OnFailure -- /bin/sh -c "sleep 4800"

# Create a CronJob based on the busybox image. Write the date to stdout every minute.
$ k run bb-cj --image=busybox --restart=OnFailure --schedule="*/1 * * * *" -- date

# Create a NGINX deployment with three replicas and create a service listening on port 80
$ k run nginx --image=nginx --replicas=3
$ k expose deploy/nginx --port=80
$ k run nginx --image=nginx --replicas=3 expose --port=80 --dry-run -o yaml > nginx.yaml
$ $ kaf nginx.yaml

# Create a WordPress pod with requests of 200m cpu, 250Mi memory and limits of 400m cpu, 500Mi memory
$ k run wordpress --image=wordpress --restart=Never --requests=cpu=200,memory=250Mi --limits=cpu=400m,memory=500Mi

# Upgrade the image version in a Deployment to nginx:1.9.7
$ k set image deploy nginx nginx=nginx:1.9.7 --record
$ k rollout status deploy nginx
$ k rollout history deploy nginx

# Add the label tier=frontend to the nginx Deployment
$ k label deploy nginx tier=frontend

# Add label while creating the Deployment
$ k run nginx --image=nginx --replicas=3 --labels=tier=frontend

# Remove the tier label from the Deployment
$ k label deploy nginx tier-

# Faster way to create a pod with env in yaml (kind of placeholder for envFrom)
$ k run cm-pod --image=nginx --restart=Never --env=place=holder --dry-run -o yaml // then change to envFrom / configMapRef etc

# Faster way to create a Service with NodePort (First create targetPort placeholder)
$ k expose deploy/np-nginx --port=80 --type=NodePort --dry-run -o yaml > np-svc.yaml // then add nodePort: xxxxx

# Confirm image used for a Pod or Deployment
$ k get deploy nginx -o yaml | grep -i image

# Run/Create quickly Deployment/Pod/Job/CronJob
$ kubectl create deployment nginx --image=nginx  #deployment
$ kubectl run nginx --image=nginx --restart=Never  #pod
$ kubectl create job nginx --image=nginx  #job
$ kubectl create cronjob nginx --image=nginx --schedule="* * * * *"  #cronJob
```


# My Notes
- Save everything using --dry-run -o yaml > xyz.yml