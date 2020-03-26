```
apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
# ReadWriteOnce – the volume can be mounted as read-write by a single node
# ReadOnlyMany – the volume can be mounted read-only by many nodes
# ReadWriteMany – the volume can be mounted as read-write by many nodes
  storageClassName: slow
  accessModes:
    - ReadWriteOnce 
  capacity:
    storage: 1Gi
  hostPath:
    path: /etc/foo
  volumeMode: Filesystem
  persistentVolumeReclaimPolicy: Recycle # Possible values: Delete/Recycle/Retain




  ###################################################


apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  storageClassName: slow
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 8Gi
  volumeMode: Filesystem

  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}
```