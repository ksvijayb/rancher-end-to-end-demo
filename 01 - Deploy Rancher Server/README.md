# Deploy Rancher Server with RKE and Helm 3

> Ensure you have completeed the setup tasks on the VM first.

## Install RKE, Kubctl, Helm and Rancher

### Test Docker Install is Present

```
docker version
```
### Install RKE CLI

Download the `RKE CLI`

```
sudo wget -O /usr/local/bin/rke \
https://github.com/rancher/rke/releases/download/v1.0.4/rke_linux-amd64
sudo chmod +x /usr/local/bin/rke
```

Next, let's validate that RKE is installed properly:

```
rke -v
```

You should have an output similar to:

`rke version v1.0.4`

### Install Kubectl

In order to interact with our Kubernetes cluster after we install it using `rke`, we need to install `kubectl`. 

The following command will add an apt repository and install `kubectl`.

```
sudo apt-get update && sudo apt-get install -y apt-transport-https
curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg \
| sudo apt-key add -
echo "deb https://apt.kubernetes.io/ kubernetes-xenial main" \
| sudo tee -a /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

After the `apt-get install -y kubectl` command finishes, we can test `kubectl` and make sure it is properly installed.

```
kubectl version --client
```

### Install Helm 3 Client

Download and Install Helm 3
```
sudo curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
sudo chmod 700 get_helm.sh
sudo ./get_helm.sh
```

Check Heml 3 Install

```
helm version --client
```

### Run RKE To Build Our Cluster

Run RKE config to configure our cluster.yaml

```
rke config
```
Lets take a look at our `cluster.yaml`
```
cat cluster.yml
```

Install Kubernetes

```
rke up
```
Show the files created..
```
ls
```

We can soft symlink the `kube_config_cluster.yml` file to our `/home/ubuntu/.kube/config` file so that `kubectl` can interact with our cluster:

```
mkdir -p /home/ubuntu/.kube
ln -s /home/ubuntu/kube_config_cluster.yml /home/ubuntu/.kube/config
```

In order to test that we can properly interact with our cluster, we can execute two commands: 

```
kubectl get nodes
```

```
kubectl get pods --all-namespaces
```
>Kubernetes Is now up and Running....

# Install Rancher Server via Helm 3

### Install Cert-Manger

Instll Cert-Manager intot he cert-manager namespace.

```
kubectl apply -f https://raw.githubusercontent.com/jetstack/cert-manager/release-0.12/deploy/manifests/00-crds.yaml
kubectl create namespace cert-manager
helm repo add jetstack https://charts.jetstack.io
helm repo update
helm install \
  cert-manager jetstack/cert-manager \
  --namespace cert-manager \
  --version v0.12.0
```

Check cert-manager pods
```
kubectl get pods --namespace cert-manager
```

### Install Rancher
```
kubectl create namespace cattle-system
helm repo add rancher-latest https://releases.rancher.com/server-charts/latest
helm repo update
```

Finally, we can install Rancher using our `helm install` command.

>Make sure to look up the server IP for the command below.

```
helm install rancher rancher-latest/rancher \
  --namespace cattle-system \
  --set hostname=rancher.<AzurePublicIPHere>.xip.io
```
### YeeHaa!!!