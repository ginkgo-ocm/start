# README

## Policy based testing

1. create a test suite implemented in ginkgo framework
2. wrap this in a container
3. TODO: push to container registry
3. support passing parameters via environment variables or options.yaml
4. TODO: wrap the container in a pod manifest

```bash
kubectl create configmap ginkgo-options --from-file=options.yaml=configure-pod-container/configmap/options.properties
```

5. TODO: load options.yaml via configmap

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: dapi-test-pod
spec:
  containers:
    - name: test-container
      image: k8s.gcr.io/busybox
      command: [ "/bin/sh", "-c", "ls /etc/config/ ; cat /etc/config/options.yaml" ]
      volumeMounts:
      - name: config-volume
        mountPath: /etc/config
  volumes:
    - name: config-volume
      configMap:
        # Provide the name of the ConfigMap containing the files you want
        # to add to the container
        name: ginkgo-options
  restartPolicy: Never
```  
(from kube examples)

  The listing should show /etc/config/options.yaml 
