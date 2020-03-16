# kubectl-rtop-pod-plugin
This repository implements a kubectl plugin for showing pod resource(`.spec.resources`)

## Details

This plugin uses
- `kubectl get --raw "/apis/metrics.k8s.io/v1beta1/pods"`
- `kubectl get pod -o json`
- `jq`

## Purpose
`kubectl top pod` doesn't output resource requests and limits.

```
NAMESPACE     POD                                NAME                      CPU(cores)   MEMORY(bytes)
kube-system   coredns-5c98db65d4-fnb58           coredns                   4m           8Mi
kube-system   coredns-5c98db65d4-trs2h           coredns                   3m           10Mi
kube-system   etcd-minikube                      etcd                      24m          34Mi
kube-system   kube-addon-manager-minikube        kube-addon-manager        11m          6Mi
kube-system   kube-apiserver-minikube            kube-apiserver            36m          173Mi 
kube-system   kube-controller-manager-minikube   kube-controller-manager   17m          38Mi
kube-system   kube-proxy-9kdv2                   kube-proxy                2m           26Mi
kube-system   kube-scheduler-minikube            kube-scheduler            1m           10Mi
kube-system   metrics-server-84bb785897-llzrt    metrics-server            0m           9Mi
kube-system   storage-provisioner                storage-provisioner       0m           36Mi
```

So, added it.

```
NAMESPACE    POD                               NAME                     CPU Usage(cores)  Request  Limit  MEMORY Usage(bytes)  Request  Limit
kube-system  coredns-5c98db65d4-fnb58          coredns                  4m                100m     -      8Mi                  70Mi     170Mi
kube-system  coredns-5c98db65d4-trs2h          coredns                  3m                100m     -      10Mi                 70Mi     170Mi
kube-system  etcd-minikube                     etcd                     24m               -        -      34Mi                 -        -
kube-system  kube-addon-manager-minikube       kube-addon-manager       11m               5m       -      6Mi                  50Mi     -
kube-system  kube-apiserver-minikube           kube-apiserver           36m               250m     -      173Mi                -        -
kube-system  kube-controller-manager-minikube  kube-controller-manager  17m               200m     -      38Mi                 -        -
kube-system  kube-proxy-9kdv2                  kube-proxy               2m                -        -      26Mi                 -        -
kube-system  kube-scheduler-minikube           kube-scheduler           1m                100m     -      10Mi                 -        -
kube-system  metrics-server-84bb785897-llzrt   metrics-server           0                 -        -      9Mi                  -        -
kube-system  storage-provisioner               storage-provisioner      0                 -        -      36Mi                 -        -
```

## Cleanup

You can "uninstall" this plugin from kubectl by simply removing it from your PATH:

    $ rm /usr/local/bin/kubectl-rtop-pod
