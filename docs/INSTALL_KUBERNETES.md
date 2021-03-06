# Kubernetes on DC/OS

Kubernetes is now available as a DC/OS package to quickly, and reliably run Kubernetes clusters on Mesosphere DC/OS.

## Known limitations

Before proceeding, please check the [current Kubernetes package limitations](https://docs.mesosphere.com/service-docs/kubernetes/1.0.2-1.9.6/limitations/).

## Pre-Requisites

Make sure your cluster fulfils the [Kubernetes package default requirements](https://docs.mesosphere.com/service-docs/kubernetes/1.0.2-1.9.6/install/#prerequisites/).

### Download command-line tools

If you haven't already, please download DC/OS client, `dcos` and Kubernetes
client, `kubectl`:

```shell
$ make get-cli
```

The `dcos` and `kubectl` binaries will be downloaded to the current workdir.
It's up to you to decided whether or not to copy or move them to another path,
e.g. a path included in `PATH`.

### Install

You are now ready to install the Kubernetes package.

For DC/OS Open cluster run:

```shell
$ make install-k8s
```

For DC/OS Enterprise cluster run:

```shell
$ make install-k8s-ee
```

The Kubernetes package installation will take place.

Wait until all tasks are running before trying to access the Kubernetes API.

You can watch the progress what was deployed so far with:

```shell
$ watch dcos kubernetes plan show deploy
```

Below is an example of how it looks like when the install ran successfully:

```
deploy (serial strategy) (COMPLETE)
├─ etcd (serial strategy) (COMPLETE)
│  ├─ etcd-0:[peer] (COMPLETE)
│  ├─ etcd-1:[peer] (COMPLETE)
│  └─ etcd-2:[peer] (COMPLETE)
├─ apiserver (parallel strategy) (COMPLETE)
│  ├─ kube-apiserver-0:[instance] (COMPLETE)
│  ├─ kube-apiserver-1:[instance] (COMPLETE)
│  └─ kube-apiserver-2:[instance] (COMPLETE)
├─ kubernetes-api-proxy (parallel strategy) (COMPLETE)
│  └─ kubernetes-api-proxy-0:[install] (COMPLETE)
├─ controller-manager (parallel strategy) (COMPLETE)
│  ├─ kube-controller-manager-0:[instance] (COMPLETE)
│  ├─ kube-controller-manager-1:[instance] (COMPLETE)
│  └─ kube-controller-manager-2:[instance] (COMPLETE)
├─ scheduler (parallel strategy) (COMPLETE)
│  ├─ kube-scheduler-0:[instance] (COMPLETE)
│  ├─ kube-scheduler-1:[instance] (COMPLETE)
│  └─ kube-scheduler-2:[instance] (COMPLETE)
├─ node (parallel strategy) (COMPLETE)
│  ├─ kube-node-0:[kube-proxy] (COMPLETE)
│  ├─ kube-node-0:[coredns] (COMPLETE)
│  ├─ kube-node-0:[kubelet] (COMPLETE)
│  ├─ kube-node-1:[kube-proxy] (COMPLETE)
│  ├─ kube-node-1:[coredns] (COMPLETE)
│  ├─ kube-node-1:[kubelet] (COMPLETE)
│  ├─ kube-node-2:[kube-proxy] (COMPLETE)
│  ├─ kube-node-2:[coredns] (COMPLETE)
│  └─ kube-node-2:[kubelet] (COMPLETE)
├─ public-node (parallel strategy) (COMPLETE)
│  ├─ kube-node-public-0:[kube-proxy] (COMPLETE)
│  ├─ kube-node-public-0:[coredns] (COMPLETE)
│  └─ kube-node-public-0:[kubelet] (COMPLETE)
└─ mandatory-addons (serial strategy) (COMPLETE)
   ├─ mandatory-addons-0:[kube-dns] (COMPLETE)
   ├─ mandatory-addons-0:[metrics-server] (COMPLETE)
   ├─ mandatory-addons-0:[dashboard] (COMPLETE)
   └─ mandatory-addons-0:[ark] (COMPLETE)
```

### Accessing the Kubernetes API

In order to access the Kubernetes API from outside the DC/OS cluster, one needs
to configure `kubectl`, the Kubernetes CLI tool:

```shell
$ dcos kubernetes kubeconfig
```

Let's test accessing the Kubernetes API and list the Kubernetes cluster nodes:

```shell
$ kubectl get nodes
NAME                                          STATUS    ROLES     AGE       VERSION
kube-node-0-kubelet.kubernetes.mesos          Ready     <none>    8m        v1.9.6
kube-node-1-kubelet.kubernetes.mesos          Ready     <none>    8m        v1.9.6
kube-node-2-kubelet.kubernetes.mesos          Ready     <none>    8m        v1.9.6
kube-node-public-0-kubelet.kubernetes.mesos   Ready     <none>    7m        v1.9.6
```

### Using kubectl proxy

For running more advanced commands such as `kubectl proxy`, an SSH tunnel is still required.
To create the tunnel, run:

```shell
$ make kubectl-tunnel
```

If `kubectl` is properly configured and the tunnel established successfully, in another terminal you should now be able to run `kubectl proxy` as well as any other command.

## Upgrading kubernetes

Check [Kubernetes upgrade doc](UPGRADE_KUBERNETES.md).

## Uninstall Kubernetes

To uninstall the DC/OS Kubernetes package run:

```shell
$ make uninstall-k8s
```

## Documentation

For more details, please check the official [Kubernetes package docs](https://docs.mesosphere.com/service-docs/kubernetes/1.0.2-1.9.6).
