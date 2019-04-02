## hostpath-provisioner

## Install

To install `hostpath-provisioner` controller:

```bash
kubectl apply -f install/.
```

You can use also `kustomize` to install the package directly as

```bash
 kustomize build install | kubectl apply -f -
```

or by reference in your overlay as:

```yaml
# kustomization.yaml
bases:
  - github.com/appwavelets/hostpath-provisioner//install?ref=master
```

The previous command will install the controller and it RBAC. Further it will install a storage class `hostpath` with `reclaimPolicy: Delete`.

Example of usage:

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: hostpath-pvc
spec:
  storageClassName: "hostpath"
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 10Gi
```

## Config

You can configure the host directory where the `hostpath-provisioner` will create the persistent volumes using the flag:

```
-pv-directory=/path/to/directory
```

**Warning:** The default value of `-pv-directory` is `/tmp/hostpath-provisioner`. Using `/tmp/...` directory could be risky for persisting volume; it will be auto-cleaned by the OS.

## Build

To build `hostpath-provisioner`:

```bash
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o hostpath-provisioner  .
```

To build the docker image:

```bash
docker build -t abdullahalmariah/hostpath-provisioner:latest -f Dockerfile.scratch .
```
