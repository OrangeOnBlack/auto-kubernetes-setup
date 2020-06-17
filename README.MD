# Auto Kubenetes Setup

Intended to be used with Ubuntu 18.04 LTS (Bionic) **Server Version**.

Installs Kubernetes 1.14 to maintain compatibility with the [Python Kubernetes API](https://github.com/kubernetes-client/python).

It can automatically:
* Install K8s itself
* Install ``WeaveNet`` as Container Network Interface/NetworkPlugin
* Install ``Metallb`` as Load-Balancer to assign IP addresses to Deployments using Services
* Install K8s Dashboard
* Enable remote access for the Docker API
* Enable Bash Autocompletion

Modify the ``metallb-config.yaml`` and run:
```
$ ./install-k8s.sh \
  --metallb-config metallb-config.yaml \
  --dashboard enable \
  --docker-remote-api enable \
  --network-adapter ens33
```
