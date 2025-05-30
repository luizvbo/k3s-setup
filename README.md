# k3s-setup

## Installing k3s 

### Network
In order to install k3s, you have to set a static IP for each the machines used
in your cluster, so that they can be reached by each other.

You can do that by setting the `/etc/network/interfaces` file or by defining a
static IP in your DHCP server.


### Installation

**Note**: If you are running k3s (either server or node) on a Raspberry Pi, you
have to to enable `cgroup` in your Raspberry Pi give that its OS don't have it
enabled by default. In your `/boot/firmware/cmdline.txt` file, append the
below:

```
cgroup_memory=1 cgroup_enable=memory
```

#### Master node 

On the master node, run the following installation command:

```shell
sudo curl -sfL https://get.k3s.io | sh -
```

You can check the installation process by running:

```shell
sudo systemctl status k3s
```

In case you have issues, make sure that `~/.kube/config` contains the proper
configuration. Otherwise, you can copy it from `/etc/rancher/k3s/k3s.yaml`:

```shell
sudo cp /etc/rancher/k3s/k3s.yaml ~/.kube/config
```

#### Worker nodes

In the worker node, you need to run the following command:

```shell
sudo curl -sfL https://get.k3s.io | K3S_URL=https://<master_node_ip>:6443 K3S_TOKEN=<your_token> sh -
```

Where `<master_node_ip>` is the IP of your master node and `<your_token>` can
be retrieved from the master node by running the command:

```shell
sudo cat /var/lib/rancher/k3s/server/node-token
```

## Helm

### Installation

In older versions of Helm (v2), there was a server-side component called Tiller
that needed to be installed in the cluster. Helm v3, which is the current
version, is client-only. This means you only need to install the helm CLI on
the machine where you will be running Helm commands.   

You can install helm by running the command:

```shell
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
chmod +x get_helm.sh
./get_helm.sh
```

### Adding Helm chart repositories

A common helm chart repository is bitnami. You can add it like this:

```shell
helm repo add bitnami https://charts.bitnami.com/bitnami
helm repo update
```

## Prefect

Add the Prefect Helm repository to Helm and list available charts and versions:

```shell
helm repo add prefect https://prefecthq.github.io/prefect-helm
helm repo update
helm search repo prefect
```
