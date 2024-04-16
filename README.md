# Kubernetes Upgrade Guide

## Overview

This guide provides step-by-step instructions for upgrading Kubernetes using `kubeadm`. Ensure you follow each step carefully to avoid any disruptions to your Kubernetes cluster.

### Find the Latest Version

To find the latest version of `kubeadm` available:

```bash
sudo apt update
sudo apt-cache madison kubeadm
```

Identify the latest version in the list in the format `1.29.x-*`, where `x` is the latest patch version.

### Update Kubernetes Components

Replace `x` with the latest patch version identified above:

```bash
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.29.x-*' && \
sudo apt-mark hold kubeadm
```

Verify that the download works and has the expected version:

```bash
kubeadm version
```

### Upgrade Kubernetes

Verify the upgrade plan:

```bash
sudo kubeadm upgrade plan
```

Replace `x` with the patch version you picked for this upgrade:

```bash
sudo kubeadm upgrade apply v1.29.x
```

### Drain Node

Before proceeding with the upgrade, drain the node to prevent disruptions:

```bash
kubectl drain <node-to-drain> --ignore-daemonsets
```

### Update kubelet and kubectl

Replace `x` with the latest patch version identified above:

```bash
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.29.x-*' kubectl='1.29.x-*' && \
sudo apt-mark hold kubelet kubectl
```

### Restart kubelet

```bash
sudo systemctl daemon-reload
sudo systemctl restart kubelet
```



Your Kubernetes cluster has now been successfully upgraded to version 1.29.x. Ensure all nodes are running the updated versions to maintain stability and security.


