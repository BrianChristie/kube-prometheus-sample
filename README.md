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

## Generate the Kubernetes Manifests from the Jsonnet:
`bash build.sh`

## Apply the Manifests
The following commands require a properly configured and authenticated KubeCtl.
Alternatively, if a automated configuration pipeline is used, the generated Manifests can be put into Git or another location to go through an automated system.

Note: When operating on multiple clusters with kubectl, be careful to have the correct context active!

```
# Update the namespace and CRDs, and then wait for them to be availble before creating the remaining resources
$ kubectl apply -f manifests/setup
$ kubectl apply -f manifests/
```
