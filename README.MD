# Auto Kubenetes Setup

Intended to be used with Ubuntu 18.04 LTS (Bionic) **Server Version**.

Installs Kubernetes 1.14 to maintain compatibility with the [Python Kubernetes API](https://github.com/kubernetes-client/python).

It can automatically

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


## Ensure Functionality after Reboot

Kubernetes does not work with swap enabled.
The setup script automatically takes care of disabling swap **temporarily -- after a system reboot swap might be enabled again**

To disable swap **temporarily** (as done by the setup script) run

```
$ sudo swapoff -a
```

To disable swap **permanently** comment out in ``/etc/fstab`` the line configuring the swapfile.
After commenting out it should look like the following (notice the line starting with ``#/swapfile``  indicating that swap is disabled because the line is commented out)

```
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=901a10af-d694-438e-9140-0844e277b7ad /               ext4    errors=remount-ro 0       1
#/swapfile                                 none            swap    sw              0       0
/dev/fd0        /media/floppy0  auto    rw,user,noauto,exec,utf8 0       0
```