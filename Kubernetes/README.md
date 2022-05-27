# [ Kubernetes (k8s) ]
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









## Prerequirements

- Docker

## Installing kubeadm, kubelet and kubectl

*[Without a package manager](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/#k8s-install-2)*

You will install these packages on all of your machines:

- `kubeadm`: the command to bootstrap the cluster.
- `kubelet`: the component that runs on all of the machines in your cluster and does things like starting pods and containers.
- `kubectl`: the command line util to talk to your cluster.

kubeadm **will not** install or manage `kubelet` or `kubectl` for you, so you will need to ensure they match the version of the Kubernetes control plane you want kubeadm to install for you. If you do not, there is a risk of a version skew occurring that can lead to unexpected, buggy behaviour. However, *one* minor version skew between the kubelet and the control plane is supported, but the kubelet version may never exceed the API server version. For example, the kubelet running 1.7.0 should be fully compatible with a 1.8.0 API server, but not vice versa.

### Install CNI plugins (required for most pod network):

```bash
CNI_VERSION="v0.8.2"
ARCH="amd64"
sudo mkdir -p /opt/cni/bin
curl -L "https://github.com/containernetworking/plugins/releases/download/${CNI_VERSION}/cni-plugins-linux-${ARCH}-${CNI_VERSION}.tgz"
tar -C /opt/cni/bin -xz
```

Define the directory to download command files

**Note:** The `DOWNLOAD_DIR` variable must be set to a writable directory. If you are running Flatcar Container Linux, set `DOWNLOAD_DIR=/opt/bin`.

```bash
DOWNLOAD_DIR=/usr/local/bin
sudo mkdir -p $DOWNLOAD_DIR
```

Install crictl (required for kubeadm / Kubelet Container Runtime Interface (CRI))

```bash
CRICTL_VERSION="v1.22.0"
ARCH="amd64"
curl -L "https://github.com/kubernetes-sigs/cri-tools/releases/download/${CRICTL_VERSION}/crictl-${CRICTL_VERSION}-linux-${ARCH}.tar.gz" | sudo tar -C $DOWNLOAD_DIR -xz
```

### Install `kubeadm`, `kubelet`, `kubectl` and add a `kubelet` systemd service:

```bash
#KUBE_RELEASE_VERSION="$(curl -sSL https://dl.k8s.io/release/stable.txt)"
KUBE_RELEASE_VERSION="v1.24.1"
KUBE_SYSTEM_SERVICE_VERSION="v0.4.0"

ARCH="amd64"

KUBEADM_BIN=~/Tools/Kubernetes/kubeadm/${KUBE_RELEASE_VERSION}/bin
KUBELET_BIN=~/Tools/Kubernetes/kubelet/${KUBE_RELEASE_VERSION}/bin

sudo mkdir -p /etc/systemd/system/kubelet.service.d
curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${KUBE_SYSTEM_SERVICE_VERSION}/cmd/kubepkg/templates/latest/deb/kubeadm/10-kubeadm.conf" | sed "s:/usr/bin:${KUBEADM_BIN}:g" | sudo tee /etc/systemd/system/kubelet.service.d/10-kubeadm.conf

curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${KUBE_SYSTEM_SERVICE_VERSION}/cmd/kubepkg/templates/latest/deb/kubelet/lib/systemd/system/kubelet.service" | sed "s:/usr/bin:${KUBELET_BIN}:g" | sudo tee /etc/systemd/system/kubelet.service
```

Enable and start `kubelet`:

```bash
systemctl enable --now kubelet
```

**Note:** The Flatcar Container Linux distribution mounts the `/usr` directory as a read-only filesystem. Before bootstrapping your cluster, you need to take additional steps to configure a writable directory. See the [Kubeadm Troubleshooting guide](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/troubleshooting-kubeadm/#usr-mounted-read-only/) to learn how to set up a writable directory.

The kubelet is now restarting every few seconds, as it waits in a crashloop for kubeadm to tell it what to do.

### Install binaries with curl on Linux

1. Go to installation directory and download the binaries version specific releases and its correspondents binaries checksum files: with the command:

   ```bash
   KUBE_ROOT=~/Tools/Kubernetes
   KUBE_SRC=${KUBE_ROOT}/src
   
   ARCH="amd64"
   KUBE_RELEASE_VERSION="v1.24.1"
   #KUBE_RELEASE_VERSION="$(curl -sSL https://dl.k8s.io/release/stable.txt)"
   
   KUBE_SYSTEM_SERVICE_VERSION="v0.4.0"
   KUBEADM_BIN=${KUBE_ROOT}/kubeadm/${KUBE_RELEASE_VERSION}/bin
   KUBELET_BIN=${KUBE_ROOT}/kubelet/${KUBE_RELEASE_VERSION}/bin
   
   mkdir -p ${KUBE_SRC}/kubeadm/${KUBE_RELEASE_VERSION}
   mkdir -p ${KUBE_SRC}/src/kubelet/${KUBE_RELEASE_VERSION}
   mkdir -p ${KUBE_SRC}/src/kubectl/${KUBE_RELEASE_VERSION}
   
   cd ${KUBE_SRC}/kubeadm/${KUBE_RELEASE_VERSION}
   curl -LO "https://dl.k8s.io/release/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubeadm"
   curl -LO "https://dl.k8s.io/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubeadm.sha256"
   
   cd ${KUBE_SRC}/kubelet/${KUBE_RELEASE_VERSION}
   curl -LO "https://dl.k8s.io/release/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubelet"
   curl -LO "https://dl.k8s.io/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubelet.sha256"
   
   cd ${KUBE_SRC}/kubectl/${KUBE_RELEASE_VERSION}
   curl -LO "https://dl.k8s.io/release/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubectl"
   curl -LO "https://dl.k8s.io/${KUBE_RELEASE_VERSION}/bin/linux/${ARCH}/kubectl.sha256"
   
   
   sudo mkdir -p /etc/systemd/system/kubelet.service.d
   curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${KUBE_SYSTEM_SERVICE_VERSION}/cmd/kubepkg/templates/latest/deb/kubeadm/10-kubeadm.conf" | sed "s:/usr/bin:${KUBEADM_BIN}:g" | sudo tee /etc/systemd/system/kubelet.service.d/10-kubeadm.conf
   
   curl -sSL "https://raw.githubusercontent.com/kubernetes/release/${KUBE_SYSTEM_SERVICE_VERSION}/cmd/kubepkg/templates/latest/deb/kubelet/lib/systemd/system/kubelet.service" | sed "s:/usr/bin:${KUBELET_BIN}:g" | sudo tee /etc/systemd/system/kubelet.service
   
   ```

2. Validate the binary (optional)

   - Validate the kubectl binary against the checksum file:

   ```bash
   cd ${KUBE_SRC}/kubeadm/${RELEASE}
   echo "$(cat kubeadm.sha256)  kubeadm" | sha256sum --check
   
   cd ${KUBE_SRC}/kubelet/${RELEASE}
   echo "$(cat kubelet.sha256)  kubelet" | sha256sum --check
   
   cd ${KUBE_SRC}/kubectl/${RELEASE}
   echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
   ```

   - If valid, the output is:

   ```console
   kubeadm: OK
   kubelet: OK
   kubectl: OK
   ```

   - If the check fails, `sha256` exits with nonzero status and prints output similar to:

   ```bash
   kubectl: FAILED
   sha256sum: WARNING: 1 computed checksum did NOT match
   ```

   **Note:** Download the same version of the binary and checksum.

   

3. Install kubectl

   - Make the binaries executables and copy to theinstallarion directory:

   ```bash
   cd ~/Tools/Kubernetes/src/${RELEASE}
   mkdir -p ~/Tools/Kubernetes/${RELEASE}/bin
   sudo install -o root -g root -m 0755 \
   kubeadm \
   kubelet \
   kubectl \
   /home/ubuntu/Tools/Kubernetes/v1.24.1/bin
   ```

   - Edit the `~/.bashrc` or `~/.zshrc` file, close and re-open the terminal

   ```bash
   # Kubernetes
   export PATH=$PATH:~/Tools/Kubernetes/v1.24.1/bin
   ```

   Reload the Terminal session or close and open a new one:

   ```bash
   source ~/.zshrc
   ```

   

4. Test to ensure the version you installed for detailed view of version:

   ```bash
   kubectl version --client --output=yaml
   ----
   clientVersion:
     buildDate: "2022-05-24T12:26:19Z"
     compiler: gc
     gitCommit: 3ddd0f45aa91e2f30c70734b175631bec5b5825a
     gitTreeState: clean
     gitVersion: v1.24.1
     goVersion: go1.18.2
     major: "1"
     minor: "24"
     platform: linux/amd64
   kustomizeVersion: v4.5.4
   ----
   ```

## [Verify kubectl configuration](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#verify-kubectl-configuration)

In order for kubectl to find and access a Kubernetes cluster, it needs a [kubeconfig file](https://kubernetes.io/docs/concepts/configuration/organize-cluster-access-kubeconfig/), which is created automatically when you create a cluster using [kube-up.sh](https://github.com/kubernetes/kubernetes/blob/master/cluster/kube-up.sh) or successfully deploy a Minikube cluster. By default, kubectl configuration is located at `~/.kube/config`.

Check that kubectl is properly configured by getting the cluster state:

```shell
kubectl cluster-info
```

If you see a URL response, kubectl is correctly configured to access your cluster.

If you see a message similar to the following, kubectl is not configured correctly or is not able to connect to a Kubernetes cluster.

```
The connection to the server <server-name:port> was refused - did you specify the right host or port?
```

For example, if you are intending to run a Kubernetes cluster on your laptop (locally), you will need a tool like Minikube to be installed first and then re-run the commands stated above.

If kubectl cluster-info returns the url response but you can't access your cluster, to check whether it is configured properly, use:

```shell
kubectl cluster-info dump
```



## Optional kubectl configurations and plugins

### Enable shell autocompletion

kubectl provides autocompletion support for Bash, Zsh, Fish, and PowerShell, which can save you a lot of typing.

Below are the procedures to set up autocompletion for Bash, Fish, and Zsh.

- [Bash](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#kubectl-autocompletion-0)

### Introduction

The kubectl completion script for Bash can be generated with the command `kubectl completion bash`. Sourcing the completion script in your shell enables kubectl autocompletion.

However, the completion script depends on [**bash-completion**](https://github.com/scop/bash-completion), which means that you have to install this software first (you can test if you have bash-completion already installed by running `type _init_completion`).

### Install bash-completion

bash-completion is provided by many package managers (see [here](https://github.com/scop/bash-completion#installation)). You can install it with `apt-get install bash-completion` or `yum install bash-completion`, etc.

The above commands create `/usr/share/bash-completion/bash_completion`, which is the main script of bash-completion. Depending on your package manager, you have to manually source this file in your `~/.bashrc` file.

To find out, reload your shell and run `type _init_completion`. If the command succeeds, you're already set, otherwise add the following to your `~/.bashrc` file:

```bash
source /usr/share/bash-completion/bash_completion
```

Reload your shell and verify that bash-completion is correctly installed by typing `type _init_completion`.

### Enable kubectl autocompletion

#### Bash

You now need to ensure that the kubectl completion script gets sourced in all your shell sessions. There are two ways in which you can do this:

- [User](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/#kubectl-bash-autocompletion-0)

```bash
echo 'source <(kubectl completion bash)' >>~/.bashrc
```

If you have an alias for kubectl, you can extend shell completion to work with that alias:

```bash
echo 'alias k=kubectl' >>~/.bashrc
echo 'complete -F __start_kubectl k' >>~/.bashrc
```

**Note:** bash-completion sources all completion scripts in `/etc/bash_completion.d`.

Both approaches are equivalent. After reloading your shell, kubectl autocompletion should be working.



#### Zsh

The kubectl completion script for Zsh can be generated with the command `kubectl completion zsh`. Sourcing the completion script in your shell enables kubectl autocompletion.

To do so in all your shell sessions, add the following to your `~/.zshrc` file:

```zsh
source <(kubectl completion zsh)
```

If you have an alias for kubectl, kubectl autocompletion will automatically work with it.

After reloading your shell, kubectl autocompletion should be working.

If you get an error like `2: command not found: compdef`, then add the following to the beginning of your `~/.zshrc` file:

```zsh
autoload -Uz compinit
compinit
```





### Install `kubectl convert` plugin

A plugin for Kubernetes command-line tool `kubectl`, which allows you to convert manifests between different API versions. This can be particularly helpful to migrate manifests to a non-deprecated api version with newer Kubernetes release. For more info, visit [migrate to non deprecated apis](https://kubernetes.io/docs/reference/using-api/deprecation-guide/#migrate-to-non-deprecated-apis)

1. Download the latest release with the command:

   ```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert"
   ```

2. Validate the binary (optional)

   Download the kubectl-convert checksum file:

   ```bash
   curl -LO "https://dl.k8s.io/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl-convert.sha256"
   ```

   Validate the kubectl-convert binary against the checksum file:

   ```bash
   echo "$(cat kubectl-convert.sha256) kubectl-convert" | sha256sum --check
   ```

   If valid, the output is:

   ```console
   kubectl-convert: OK
   ```

   If the check fails, `sha256` exits with nonzero status and prints output similar to:

   ```bash
   kubectl-convert: FAILED
   sha256sum: WARNING: 1 computed checksum did NOT match
   ```

   **Note:** Download the same version of the binary and checksum.

3. Install kubectl-convert

   ```bash
   sudo install -o root -g root -m 0755 kubectl-convert /usr/local/bin/kubectl-convert
   ```

4. Verify plugin is successfully installed

   ```shell
   kubectl convert --help
   ```

   If you do not see an error, it means the plugin is successfully installed.

















