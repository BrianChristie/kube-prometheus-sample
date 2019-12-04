# How to use kube-prometheus

For more details see upstream: https://github.com/coreos/kube-prometheus

## Install prerequisites:

### Install GoLang:
`brew install go`

### Add Go installed binaries to your path
You may wish to add this to ~/.bash_profile (on Mac)
`export PATH=$PATH:$(go env GOPATH)/bin`

### Install Jsonnet
`go get github.com/google/go-jsonnet/cmd/jsonnet`

### Install gojsontoyaml
`go get github.com/brancz/gojsontoyaml`

### Install Jsonnet Bundler: 
`GO111MODULE="on" go get github.com/jsonnet-bundler/jsonnet-bundler/cmd/jb`

### VSCode
If using VSCode
- Install the Jsonnet plugin: https://marketplace.visualstudio.com/items?itemName=heptio.jsonnet
- Edit settings.json to point at the directory where you've cloned this repository. Unfortunately VSCode currently doesn't allow using `${workspaceFolder}` in settings.json. 

## Generate the Kubernetes Manifests from the Jsonnet:
`bash build.sh`

## Customize `main.jsonnet`

### PersistentVolumeClaims for Prometheus Storage
This configuration is setup to use a PersistentVolumeClaim. This requires that the cluster
be configured with a PVC class. The class name is set on the line
`pvc.mixin.spec.withStorageClassName('ssd'),`. This can be changed to match the PVC class name that the cluster supports.

### Ingress
https://github.com/coreos/kube-prometheus/blob/master/docs/exposing-prometheus-alertmanager-grafana-ingress.md



## Apply the Manifests
The following commands require a properly configured and authenticated KubeCtl.
Alternatively, if a automated configuration pipeline is used, the generated Manifests can be put into Git or another location to go through an automated system.

Note: When operating on multiple clusters with kubectl, be careful to have the correct context active!

```
# Update the namespace and CRDs, and then wait for them to be availble before creating the remaining resources
$ kubectl apply -f manifests/setup
$ kubectl apply -f manifests/
```
