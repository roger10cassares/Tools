# [ Kubernetes (k8s) ]

## Prerequirements

- Docker



# 1. Install kubeadm

*From: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/*

## 1.1. Before you begin

- A compatible Linux host. The Kubernetes project provides generic instructions for Linux distributions based on Debian and Red Hat, and those distributions without a package manager.
- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node. See [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#verify-mac-address) for more details.
- Certain ports are open on your machines. See [here](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#check-required-ports) for more details.
- Swap disabled. You **MUST** disable swap in order for the kubelet to work properly.



### 1.1.1. Verify the MAC address and product_uuid are unique for every node

- You can get the MAC address of the network interfaces using the command 

  ```bash
  ip link
  # or
  ifconfig -a
  ```

- The `product_uuid` can be checked by using the command

  ```bash
  sudo cat /sys/class/dmi/id/product_uuid
  ```

  It is very likely that hardware devices will have unique addresses, although some virtual machines may have identical values. Kubernetes uses these values to uniquely identify the nodes in the cluster. If these values are not unique to each node, the installation process may [fail](https://github.com/kubernetes/kubeadm/issues/31).



## Check network adapters

If you have more than one network adapter, and your Kubernetes components are not reachable on the default route, we recommend you add IP route(s) so Kubernetes cluster addresses go via the appropriate adapter.



## Check required ports

These [required ports](https://kubernetes.io/docs/reference/ports-and-protocols/) need to be open in order for Kubernetes components to communicate with each other. You can use tools like netcat to check if a port is open. For example:

```shell
nc 127.0.0.1 6443
```

The pod network plugin you use may also require certain ports to be open. Since this differs with each pod network plugin, please see the documentation for the plugins about what port(s) those need.





## Installing a container runtime

To run containers in Pods, Kubernetes uses a [container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes).

By default, Kubernetes uses the [Container Runtime Interface](https://kubernetes.io/docs/concepts/overview/components/#container-runtime) (CRI) to interface with your chosen container runtime.

If you don't specify a runtime, kubeadm automatically tries to detect an installed container runtime by scanning through a list of known endpoints.

If multiple or no container runtimes are detected kubeadm will throw an error and will request that you specify which one you want to use.

See [container runtimes](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) for more information.

**Note:** Docker Engine does not implement the [CRI](https://kubernetes.io/docs/concepts/architecture/cri/) which is a requirement for a container runtime to work with Kubernetes. For that reason, an additional service [cri-dockerd](https://github.com/Mirantis/cri-dockerd) has to be installed. cri-dockerd is a project based on the legacy built-in Docker Engine support that was [removed](https://kubernetes.io/dockershim) from the kubelet in version 1.24.

The tables below include the known endpoints for supported operating systems:

- [Linux](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#container-runtime-0)

| Runtime                           | Path to Unix domain socket                   |
| --------------------------------- | -------------------------------------------- |
| containerd                        | `unix:///var/run/containerd/containerd.sock` |
| CRI-O                             | `unix:///var/run/crio/crio.sock`             |
| Docker Engine (using cri-dockerd) | `unix:///var/run/cri-dockerd.sock`           |



***Note**: See Docker Installation Steps at https://github.com/roger10cassares/Tools/Docker*











# Container Runtimes

## Install and configure prerequisites

The following steps apply common settings for Kubernetes nodes on Linux.

You can skip a particular setting if you're certain you don't need it.

For more information, see [Network Plugin Requirements](https://kubernetes.io/docs/concepts/extend-kubernetes/compute-storage-net/network-plugins/#network-plugin-requirements) or the documentation for your specific container runtime.

### Forwarding IPv4 and letting iptables see bridged traffic

Execute the below mentioned instructions:

```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

# sysctl params required by setup, params persist across reboots
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

# Apply sysctl params without reboot
sudo sysctl --system
```

Verify that the `br_netfilter`, `overlay` modules are loaded by running below instructions:

```bash
lsmod | grep br_netfilter
lsmod | grep overlay
```

Verify that the `net.bridge.bridge-nf-call-iptables`, `net.bridge.bridge-nf-call-ip6tables`, `net.ipv4.ip_forward` system variables are set to 1 in your `sysctl` config by running below instruction:

```bash
sysctl net.bridge.bridge-nf-call-iptables net.bridge.bridge-nf-call-ip6tables net.ipv4.ip_forward
----
call-ip6tables net.ipv4.ip_forward
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
```

## Cgroup drivers

On Linux, [control groups](https://kubernetes.io/docs/reference/glossary/?all=true#term-cgroup) are used to constrain resources that are allocated to processes.

Both [kubelet](https://kubernetes.io/docs/reference/generated/kubelet) and the underlying container runtime need to interface with control groups to enforce [resource management for pods and containers](https://kubernetes.io/docs/concepts/configuration/manage-resources-containers/) and set resources such as cpu/memory requests and limits. To interface with control groups, the kubelet and the container runtime need to use a *cgroup driver*. It's critical that the kubelet and the container runtime uses the same cgroup driver and are configured the same.

There are two cgroup drivers available:

- [`cgroupfs`](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cgroupfs-cgroup-driver)
- [`systemd`](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#systemd-cgroup-driver)



### systemd cgroup driver

When [systemd](https://www.freedesktop.org/wiki/Software/systemd/) is chosen as the init system for a Linux distribution, the init process generates and consumes a root control group (`cgroup`) and acts as a cgroup manager.

systemd has a tight integration with cgroups and allocates a cgroup per systemd unit. As a result, if you use `systemd` as the init system with the `cgroupfs` driver, the system gets two different cgroup managers.

Two cgroup managers result in two views of the available and in-use resources in the system. In some cases, nodes that are configured to use `cgroupfs` for the kubelet and container runtime, but use `systemd` for the rest of the processes become unstable under resource pressure.

The approach to mitigate this instability is to use `systemd` as the cgroup driver for the kubelet and the container runtime when systemd is the selected init system.

To set `systemd` as the cgroup driver, edit the [`KubeletConfiguration`](https://kubernetes.io/docs/tasks/administer-cluster/kubelet-config-file/) option of `cgroupDriver` and set it to `systemd`. For example:

```yaml
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
...
cgroupDriver: systemd
```

If you configure `systemd` as the cgroup driver for the kubelet, you must also configure `systemd` as the cgroup driver for the container runtime. Refer to the documentation for your container runtime for instructions. For example:

- [containerd](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#containerd-systemd)
- [CRI-O](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#cri-o)



## Container runtimes

**Note:** This section links to third party projects that provide functionality required by Kubernetes. The Kubernetes project authors aren't responsible for these projects, which are listed alphabetically. To add a project to this list, read the [content guide](https://kubernetes.io/docs/contribute/style/content-guide/#third-party-content) before submitting a change. [More information.](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#third-party-content-disclaimer)

### containerd

This section outlines the necessary steps to use containerd as CRI runtime.

Use the following commands to install Containerd on your system:

Follow the instructions for [getting started with containerd](https://github.com/containerd/containerd/blob/main/docs/getting-started.md). Return to this step once you've created a valid configuration file, `config.toml`.

- [Linux](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#finding-your-config-toml-file-0)
- [Windows](https://kubernetes.io/docs/setup/production-environment/container-runtimes/#finding-your-config-toml-file-1)

```


```



You can find this file under the path `/etc/containerd/config.toml`.

On Linux the default CRI socket for containerd is `/run/containerd/containerd.sock`. On Windows the default CRI endpoint is `npipe://./pipe/containerd-containerd`.

#### Configuring the `systemd` cgroup driver

```bash
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1
sudo sed -i 's/SystemdCgroup \= false/SystemdCgroup \= true/g' /etc/containerd/config.toml
```

Verify the `systemd` cgroup driver in `/etc/containerd/config.toml` with `runc`:

```
[plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  ...
  [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
```



If you apply this change, make sure to restart containerd:

```shell
sudo systemctl restart containerd
```





## Installing kubeadm, kubelet and kubectl

*[With a package manager](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#k8s-install-2)*

You will install these packages on all of your machines:

- `kubeadm`: the command to bootstrap the cluster.
- `kubelet`: the component that runs on all of the machines in your cluster and does things like starting pods and containers.
- `kubectl`: the command line util to talk to your cluster.

kubeadm **will not** install or manage `kubelet` or `kubectl` for you, so you will need to ensure they match the version of the Kubernetes control plane you want kubeadm to install for you. If you do not, there is a risk of a version skew occurring that can lead to unexpected, buggy behaviour. However, *one* minor version skew between the kubelet and the control plane is supported, but the kubelet version may never exceed the API server version. For example, the kubelet running 1.7.0 should be fully compatible with a 1.8.0 API server, but not vice versa.

1. Update the `apt` package index and install packages needed to use the Kubernetes `apt` repository:

   ```shell
   sudo apt-get update
   sudo apt-get install -y apt-transport-https ca-certificates curl
   ```

2. Download the Google Cloud public signing key:

   ```shell
   sudo curl -fsSLo /etc/apt/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
   ```

3. Add the Kubernetes `apt` repository:

   ```shell
   echo "deb [signed-by=/etc/apt/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
   ```

4. Update `apt` package index, install kubelet, kubeadm and kubectl, and pin their version:



```bash
apt-cache madison kubelet | awk '{ print $3 }'
1.26.0-00
1.25.5-00
1.25.4-00
1.25.3-00
1.25.2-00
1.25.1-00
1.25.0-00

apt-cache madison kubeadm | awk '{ print $3 }'
----
1.26.0-00
1.25.5-00
1.25.4-00
1.25.3-00
1.25.2-00
1.25.1-00
1.25.0-00

apt-cache madison kubectl | awk '{ print $3 }'
----
1.26.0-00
1.25.5-00
1.25.4-00
1.25.3-00
1.25.2-00
1.25.1-00
1.25.0-00
```





```shell
KUBELET_VERSION_STRING=1.26.0-00
KUBEADM_VERSION_STRING=1.26.0-00
KUBECTL_VERSION_STRING=1.26.0-00

sudo apt-get update
sudo apt-get install -y kubelet=$KUBELET_VERSION_STRING kubeadm=$KUBEADM_VERSION_STRING kubectl=$KUBECTL_VERSION_STRING
sudo apt-mark hold kubelet kubeadm kubectl
```

**Note:** In releases older than Debian 12 and Ubuntu 22.04, `/etc/apt/keyrings` does not exist by default. You can create this directory if you need to, making it world-readable but writeable only by admins.

The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.



```
sudo kubeadm init --control-plane-endpoint=master
```



mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config



kubectl cluster-info
Kubernetes control plane is running at https://master:6443
CoreDNS is running at https://master:6443/api/v1/namespaces/kube-system/services/kube-dns:dns/proxy

To further debug and diagnose cluster problems, use 'kubectl cluster-info dump'.
ubuntu@master:~$ kubectl get nodes
NAME     STATUS     ROLES           AGE    VERSION
master   NotReady   control-plane   5m1s   v1.26.0



### Install CNI plugins (required for most pod network):

curl https://raw.githubusercontent.com/projectcalico/calico/v3.24.5/manifests/calico.yaml -O

kubectl apply -f calico.yaml





