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
alias kn='k config set-context --current --namespace ' // you can just do `kn np1` to switch to namespace. Mind the space

alias kcf='k create -f'
alias kaf='k apply -f'
alias kdgf='k delete --grace-period=0 --force'
alias kdgff='k delete --grace-period=0 --force -f'

alias kdp='k describe pod'
alias kgp='k get pod'
alias kgpyml='kgp -o yaml '
alias kryml= 'kubectl run -o yaml --dry-run'


## Probably wont use it
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

# PODS
## Create a NGINX pod
$ k run nginx --image=nginx --restart=Never
$ k run nginx --image=nginx --restart=Never  -l abc=xyz,aaa=bbb  --command "/bin/sh -c" "sleep 4800" --env DB_HOST=sql01 --env DB_UN=user --port=80 -n default  --requests=cpu=200m,memory=250Mi --limits=cpu=400m,memory=500Mi --serviceaccount=myuser   // then change to envFrom / configMapRef etc

# Deploy / Service
## Create a NGINX deployment with 3 replicas
$ k run nginx --image=nginx --replicas=3
# Faster way to create a Service with NodePort (First create targetPort placeholder)
$ k expose deploy/np-nginx --port=80 --type=NodePort --dry-run -o yaml > np-svc.yaml // then add nodePort: xxxxx

# ConfigMap/Secret
$ k create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
$ k create secret generic app-secret --from-literal=DB_HOST=sql01


## Create a NGINX deployment with three replicas and create a service listening on port 80
$ k expose deploy/nginx --port=80
$ k run nginx --image=nginx --replicas=3 expose --port=80 --dry-run -o yaml > nginx.yaml
$ kaf nginx.yaml

# JOB
## Create a Job based on the busybox image. Execute the command "sleep 4800"
$ k run bb-job --image=busybox --restart=OnFailure -- /bin/sh -c "sleep 4800"

# CronJob
## Create a CronJob based on the busybox image. Write the date to stdout every minute.
$ k run bb-cj --image=busybox --restart=OnFailure --schedule="*/1 * * * *" -- date

# Upgrade the image version in a Deployment to nginx:1.9.7
## set image of deployement
$ k set image deploy/nginx nginx=nginx:1.9.7 --record
## set image of pod
$ k set image pod/nginx nginx=nginx:1.9.7
$ k rollout status deploy/nginx-deployment
$ k rollout history deploy/nginx-deployment
## Get more info about revision
$ k rollout history deploy/nginx-deployment --revision=2
## Rollback to previous revision
$ k rollout undo deploy/nginx-deployment
## Rollback to specific revision
$ k rollout undo deploy/nginx-deployment --to-revision=3


# Labels
## Add the label tier=frontend to the nginx Deployment
$ k label deploy nginx tier=frontend abc=xyz
$ k label po nginx-7bb7cd8db5-hk758 abc=ccc def=ooo
## Add label while creating the Deployment
$ k run nginx --image=nginx --replicas=3 -l tier=frontend,567=74 --dry-run -o yaml
## Remove the tier label from the Deployment
$ k label deploy nginx tier-
## get pod based on labels
$ kgp --show-labels
$ kgp -l tier=backend,abc=ccc
$ kgp -l tier!=backend,abc=ccc
$ kgp -l 'tier not in (backend,frontend)'


# TAINT
$ k taint nodes <node-name> key=value:taint-effect

# Logging 
$ k logs <podname> -c <oneofthecontainername> # multiple container

# Monitoring
$ k top nodes
$ k top pods
$ k top pod <pod-name>

# Run/Create quickly Deployment/Pod/Job/CronJob/namespace (ns)/configMap (cm) / Resources
$ kubectl create deployment nginx --image=nginx  #deployment
$ kubectl run nginx --image=nginx --restart=Never  #pod
$ kubectl create job nginx --image=nginx  #job
$ kubectl create cronjob nginx --image=nginx --schedule="* * * * *"  #cronJob
$ k create ns abc -o yaml --dry-run
$ k create configmap my-config --from-literal=key1=config1 --from-literal=key2=config2
$ k create quota myrq --hard=cpu=1,memory=1G,pods=2 --dry-run -o yaml


# Some special cases
$ k create cm configmap4 --from-file=special=config4.txt
$ k exec -it <podname> <oneofthecontainername> -- ls # multiple container


```


# My Notes
- Save everything using --dry-run -o yaml > xyz.yml