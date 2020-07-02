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


## Add Nodes to the Master (Multi-Node Setup)

Prerequisites on the slave nodes

```
$ curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
$ echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
$ sudo apt-get update
$ sudo apt-get install -y kubelet=1.14.8-00 kubeadm=1.14.8-00 kubectl=1.14.8-00
$ sudo apt-mark hold kubelet kubeadm kubectl
```

Generate the *join-command* on the master node

```
$ kubeadm token create --print-join-command
```

Example output of the above command

```
kubeadm join 131.234.28.139:6443 --token y784fc.hgr6uxqkmipg52xp --discovery-token-ca-cert-hash sha256:ce50c08ec52543833784c6946c91c95507ba094bca7335c991f65676ae1e859e
```

Run the *join-command* (``kubeadm join ...``) on each slave node
