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
POD                 NAME                CPU(cores)   MEMORY(bytes)   
dns-default-jk24b   dns                 2m           15Mi            
dns-default-jk24b   dns-node-resolver   0m           5Mi            
```

So, added it.

```
NAME               Container          CPU Usage  Request  Limit  Memory Usage  Request  Limit
dns-default-jk24b  dns                2m         100m     -      15Mi          70Mi     512Mi
dns-default-jk24b  dns-node-resolver  0          10m      -      5Mi           -        -
```

## Cleanup

You can "uninstall" this plugin from kubectl by simply removing it from your PATH:

    $ rm /usr/local/bin/kubectl-rtop-pod
