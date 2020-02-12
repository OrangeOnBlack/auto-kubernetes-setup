# Auto Kubenetes Setup

It can automatically setup:
* K8 itself
* WeaveNet
* Metallb
* K8 Dashboard
* Enable Docker API for remote access


Modify the ``metallb-config.yaml`` and run:
```
$ ./install-k8.sh --metallb-config metallb-config.yaml --dashboard enable --docker-remote-api enable --network-adapter ens33
```