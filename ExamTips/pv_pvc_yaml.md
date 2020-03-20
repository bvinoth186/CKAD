apiVersion: v1
kind: PersistentVolume
metadata:
  name: pv0003
spec:
  capacity:
    storage: 1Gi
  volumeMode: Filesystem
# ReadWriteOnce – the volume can be mounted as read-write by a single node
# ReadOnlyMany – the volume can be mounted read-only by many nodes
# ReadWriteMany – the volume can be mounted as read-write by many nodes
  accessModes:
    - ReadWriteOnce 
  persistentVolumeReclaimPolicy: Recycle # Possible values: Delete/Recycle 
  storageClassName: slow



  ###################################################


apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: myclaim
spec:
  accessModes:
    - ReadWriteOnce
  volumeMode: Filesystem
  resources:
    requests:
      storage: 8Gi
  storageClassName: slow
  selector:
    matchLabels:
      release: "stable"
    matchExpressions:
      - {key: environment, operator: In, values: [dev]}