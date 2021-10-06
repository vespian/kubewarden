# Install:
```
helm repo add kubewarden https://charts.kubewarden.io
helm repo add jetstack https://charts.jetstack.io
helm repo update

kind create cluster --name kubewarden-test --config kind-config.yaml

helm install --wait cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.5.3 --set installCRDs=true
helm install --create-namespace -n kubewarden kubewarden-crds kubewarden/kubewarden-crds
helm install --wait -n kubewarden kubewarden-controller kubewarden/kubewarden-controller

k get pods -A
```

# Deploy manifests
```
k apply -f ./manifests/
kubectl wait --for=condition=policyactive --timeout=600s ClusterAdmissionPolicy/pod-palidrome

k get validatingwebhookconfigurations.admissionregistration.k8s.io

curl -vI https://f002.backblazeb2.com/file/ves-public/pod-palidrome.wasm
```

```
$ k apply -f good-pod.yml
pod/hello-world created
```

```
$ k apply -f bad-pod.yml
Error from server: error when creating "bad-pod.yml": admission webhook "pod-palidrome.kubewarden.admission" denied the request: The 'level' label is a palidrome
```

# Cleanup
```
kind delete cluster --name kubewarden-test
```
