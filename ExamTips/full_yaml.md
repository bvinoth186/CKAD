apiVersion: v1 # For Pod,service,replicationController, Namespace, configMap, Secret
kind: Pod
metadata:
  annotations:
    commit: 232n48dnfud0b0b0gi
  labels:
    aaa: bbb
    abc: xyz
  name: nginx
  namespace: default
spec:
  volumes:
  - name: default-token-9zkp5
    secret:
      secretName: default-token-9zkp5
  - name: myvolume # just a name, you'll reference this in the pods
    configMap:
      name: cmvolume # name of your configmap    
  - name: myawesomevolume
    hostPath:
      path: /etc/foo
      type: Directory  
  - name: volumeformultinodes
    awsElasticBlockStore:
      volumeID: <volumeID>
      fsType: ext4
  - name: pvc-my-storage
    persistentVolumeClaim:  # this has to be created
      claimName: mypvc
  serviceAccountName: default
  securityContext: 
    runAsUser: 1010 
  strategy:
    rollingUpdate:
      maxSurge: 3
      maxUnavailable: 2
    type: RollingUpdate
  containers:
  - image: nginx
    name: nginx
    securityContext: 
      runAsUser: 1010
      capabilities:
        add: ["MAC_ADMIN"] 
    volumeMounts:
    - mountPath: /var/run/secrets/kubernetes.io/serviceaccount
      name: default-token-9zkp5
    - name: myvolume
      mountPath: /etc/lala
 
    command:
    - /bin/sh -c
    - sleep 4800
    args: ["sleep", "4800"]
    ports:
    - containerPort: 80
      protocol: TCP
    resources:
      limits:
        cpu: 400m
        memory: 500Mi
      requests:
        cpu: 200m
        memory: 250Mi
    env:
    - name: DB_HOST
      value: sql01
    - name: DB_UN
      value: user      
 #  envFrom:
 #    - secretRef: #configMapRef
 #        name: app-secret
 #  env:
 #    - name: DB_HOST
#       valueFrom:
#         secretKeyRef: # configMapKeyRef
#           name: app-secret
#           key: DB_HOST

  # livenessProbe:
    readinessProbe:
      httpGet:
        path: /authn/health
        port: 3000
      exec:
        command:
          - cat
          - /app/is_healthy
      tcpSocket:
        port: 3306
      initialDelaySeconds: 10
      periodSeconds: 10
      timeoutSeconds: 5
      failureThreshold: 8
      
  restartPolicy: Never
  nodeSelector:  # kubectl label nodes node-01 size=Large
    size: Large
  tolerations: # kubectl taint nodes <node-name> key=value:taint-effect
  - effect: NoExecute
    key: node.kubernetes.io/not-ready
    operator: Exists
    tolerationSeconds: 300
##### requiredDuringSchedulingIgnoredDuringExecution:  pod must have affinity to put it on a node(during scheduling). If node is already in pod, ignore
##### preferredDuringSchedulingIgnoredDuringExecution: pod may/may not have affinity to put it on a node (during scheduling) and can be placed to tainted node even no label assigned
##### requiredDuringSchedulingRequiredDuringExecution : pod must have affinity both the times    
  affinity:
    nodeAffinity:
      requiredDuringSchedulingIgnoredDuringExecution:
        nodeSelectorTerms:
        - matchExpressions:
          - key: size
            operator: In  // can be NotIn,Exists
            values:
            - Large       