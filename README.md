# kubernetes-cluster-ansible

An Ansible playbook for creating an on-premises Kubernetes cluster.
Tested on Kubernetes v1.29 and Cilium 1.14.5 on Debian 12.

I wanted to create a cluster based on my own preferences, using modern components.

## Features
* Debian 12 as host operating system
* [crun](https://github.com/containers/crun) container runtime
* [CRI-O](https://github.com/cri-o/cri-o) Kubernetes Container Runtime Interface
* [Cilium](https://cilium.io/) CNI plugin with kube-proxy replacement enabled

## Requirements
* One master node
* One or more worker nodes
* All nodes running Debian 12
* SSH access to the nodes
* Full network connectivity between the nodes
* Ansible

## Instructions
1. Specify the IP addresses of your nodes in the `inventory.ini` file
2. Specify the Kubernetes and Cilium versions in the `group_vars/all.yaml` file
3. Choose a Pod CIDR for your pods (must not overlap with any other networks)
4. Run the playbook

        ansible-playbook -i inventory.yaml playbook.yaml

5. Cluster should be created and running. `kubectl` should be working on the master node now. 

## Roles explained
* common_install
  * Upgrade packages, install useful system utilities
* kube_preinstall
  * Install container tools and kubeadm on every node
  * Configure the system for Kubernetes
* master_install
  * Create the cluster
* worker_install
  * Join worker nodes to the cluster
* cilium_install
  * Install Cilium CNI plugin using Helm