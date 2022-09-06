# Conformance tests for kubeasy kubernetes cluster

## Node Provisioning

Provision 2 nodes for your cluster (OS: CentOS 7.9.2009)

1 master node (4c4g)

2 worker node (4c4g)

for a High-Availability Kubernetes Cluster, read [more](https://github.com/buxiaomo/kubeasy/blob/main/group_vars/README.md)

## Install the cluster

(1) Download 'kubeasy' code

```
yum install git make vim -y
git clone -b v1.18 https://github.com/buxiaomo/kubeasy.git /usr/local/src/kubeasy
cd /usr/local/src/kubeasy
make runtime
make -C group_vars/
cat inventory/hosts
[master]
192.168.56.11

[worker]
192.168.56.12
192.168.56.13

[kubernetes:children]
master
worker

[all:vars]
ansible_python_interpreter=/usr/bin/python3
ansible_ssh_port=22
ansible_ssh_user=root
# ansible_ssh_pass=root
# ansible_sudo_user=root
# ansible_sudo_pass=root
```

(2) install kubernetes

```
cd /usr/local/src/kubeasy
make deploy DOWNLOAD_WAY=official
```

(3) Add a worker node

```
cd /usr/local/src/kubeasy
make scale
```

## Run Conformance Test

The standard tool for running these tests is
[Sonobuoy](https://github.com/heptio/sonobuoy).  Sonobuoy is 
regularly built and kept up to date to execute against all 
currently supported versions of kubernetes.

Run conformance tests

```
cd /usr/local/src/kubeasy
make e2e
```

View actively running pods:

```
sonobuoy status 
```

To inspect the logs:

```
sonobuoy logs
```