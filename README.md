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

## Running

```sh
# assumes you have a working KUBECONFIG
$ GO111MODULE="on" go build cmd/kubectl-ns.go
# place the built binary somewhere in your PATH
$ cp ./kubectl-ns /usr/local/bin

# you can now begin using this plugin as a regular kubectl command:
# update your configuration to point to "new-namespace"
$ kubectl ns new-namespace
# any kubectl commands you perform from now on will use "new-namespace"
$ kubectl get pod
NAME                READY     STATUS    RESTARTS   AGE
new-namespace-pod   1/1       Running   0          1h

# list all of the namespace in use by contexts in your KUBECONFIG
$ kubectl ns --list

# show the namespace that the currently set context in your KUBECONFIG points to
$ kubectl ns
```

## Use Cases

This plugin can be used as a developer tool, in order to quickly view or change the current namespace
that kubectl points to.

It can also be used as a means of showcasing usage of the cli-runtime set of utilities to aid in
third-party plugin development.

## Cleanup

You can "uninstall" this plugin from kubectl by simply removing it from your PATH:

    $ rm /usr/local/bin/kubectl-rtop-pod
