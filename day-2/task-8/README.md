# Task-8: Volume Mount Directory from the Node File-System
1. Add label to one of the nodes using **kubectl label node kube-node-1 app=letschat** command
2. Create some directory for mongofb inside the node 
  > * You can use `kube-ssh kube-node-1`
3. Add nodeSelector to the Lets-Chat-DB Deployment and volume to the hostPath
  > * The mountPath for persisting mongodb should be /data/db
4. Check in Browser, even after restart pod User is persistent

  
### Specifications Examples
#### pod-with-hostPath.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: test-pd
spec:
  containers:
  - image: k8s.gcr.io/test-webserver
    name: test-container
    volumeMounts:
    - mountPath: /test-pd
      name: test-volume
  volumes:
  - name: test-volume
    hostPath:
      # directory location on host
      path: /data
      # this field is optional
      type: Directory
```
#### pod-with-node-selector.yaml
```yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    env: test
spec:
  containers:
  - name: nginx
    image: nginx
    imagePullPolicy: IfNotPresent
  nodeSelector:
    disktype: ssd
```
#### secret.yaml
First get the values in base64:
```bash
$ echo -n "admin" | base64
YWRtaW4=
$ echo -n "1f2d1e2e67df" | base64
MWYyZDFlMmU2N2Rm
```