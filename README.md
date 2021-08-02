# minikubelab

`minikube` starter project for **Ubuntu 18.04** on **Windows Subsystem Linux 2**.

<!-- TOC -->

- [minikubelab](#minikubelab)
  - [TL;DR](#tldr)
  - [Project](#project)
    - [Installing minikube](#installing-minikube)
    - [Configuring VM driver](#configuring-vm-driver)
    - [Starting minikube](#starting-minikube)
    - [minikube Commands](#minikube-commands)
    - [minikube Addons](#minikube-addons)
      - [Dashboard Addon](#dashboard-addon)
      - [Installing Nginx to access dashboard on a headless VM](#installing-nginx-to-access-dashboard-on-a-headless-vm)
    - [kubectl Commands](#kubectl-commands)
    - [Troubleshooting](#troubleshooting)
    - [References](#references)

<!-- /TOC -->

## TL;DR

`minikube` is local **Kubernetes**, focusing on making it easy to learn and develop for **Kubernetes**.

## Project

### Installing minikube

What you'll need:
* 2 CPUs or more
* 2Gb of free memory
* 20Gb of free disk space
* Internet connection
* Container or virtual machine manager

> All you need is **Docker** (or similarly compatible) container or a Virtual Machine environment, and **Kubernetes** is a single command away: `minikube start`.

```
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube_latest_amd64.deb
sudo dpkg -i minikube_latest_amd64.deb
```

### Configuring VM driver

As we're on a virtual machine, we should set VM driver to none, as we cannot virtualize on virtualization.

```
sudo minikube config set vm-driver none
```

You should see the following output, which you can ignore as we don't have `minikube` running.

```
These changes will take effect upon a minikube delete and then a minikube start.
```

### Starting minikube

First, change permissions for your `$USER` to the `.minikube` directory.

```
sudo chown -R $USER $HOME/.minikube; chmod -R u+wrx $HOME/.minikube
```

Then, start `minikube` using the following command:

```
minikube start --driver=docker --delete-on-failure
```

_Warning: The option `--driver=none` should not be used in Windows._

A successful output should have the following:

```
😄  minikube v1.20.0 on Ubuntu 18.04
✨  Using the docker driver based on user configuration
👍  Starting control plane node minikube in cluster minikube
🚜  Pulling base image ...
💾  Downloading Kubernetes v1.20.2 preload ...
    > preloaded-images-k8s-v10-v1...: 491.71 MiB / 491.71 MiB  100.00% 7.71 MiB
    > gcr.io/k8s-minikube/kicbase...: 358.09 MiB / 358.10 MiB  100.00% 5.30 MiB
    > gcr.io/k8s-minikube/kicbase...: 358.10 MiB / 358.10 MiB  100.00% 5.90 MiB
🔥  Creating docker container (CPUs=2, Memory=2200MB) ...
🐳  Preparing Kubernetes v1.20.2 on Docker 20.10.6 ...
    ▪ Generating certificates and keys ...
    ▪ Booting up control plane ...
    ▪ Configuring RBAC rules ...
🔎  Verifying Kubernetes components...
    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5
🌟  Enabled addons: storage-provisioner, default-storageclass
🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

### minikube Commands

`minikube start` - Starts a new `minikube` cluster
`minikube stop` - Stops the cluster and all its containers
`minikube delete` - Deletes the cluster
`minikube image list` - Displays a list of images
`minikube image load` - Downloads and adds an image
`minikube ip` - Gets the cluster IP
`minikube logs` - Show the cluster logs
`minikube service list` - Displays a list of services
`minikube version` - Displays version
`minikube update-check` - Checks latest version

_Note: The `minikube image` command has superceded `minikube cache`._

### minikube Addons

`minikube addons` are preconfigured and preinstalled items that you can either enable or disable to get the functionality that you want from these addons.

`minikube addons list` - Displays a list of addons

#### Dashboard Addon

```
minikube addons enable dashboard
```

```
    ▪ Using image kubernetesui/dashboard:v2.1.0
    ▪ Using image kubernetesui/metrics-scraper:v1.0.4
💡  Some dashboard features require the metrics-server addon. To enable all features please run:

        minikube addons enable metrics-server


🌟  The 'dashboard' addon is enabled
```

```
minikube addons enable metrics-server
```

```
|-----------------------------|----------|--------------|
|         ADDON NAME          | PROFILE  |    STATUS    |
|-----------------------------|----------|--------------|
| ambassador                  | minikube | disabled     |
| auto-pause                  | minikube | disabled     |
| csi-hostpath-driver         | minikube | disabled     |
| dashboard                   | minikube | enabled ✅   |
| default-storageclass        | minikube | enabled ✅   |
| efk                         | minikube | disabled     |
| freshpod                    | minikube | disabled     |
| gcp-auth                    | minikube | disabled     |
| gvisor                      | minikube | disabled     |
| helm-tiller                 | minikube | disabled     |
| ingress                     | minikube | disabled     |
| ingress-dns                 | minikube | disabled     |
| istio                       | minikube | disabled     |
| istio-provisioner           | minikube | disabled     |
| kubevirt                    | minikube | disabled     |
| logviewer                   | minikube | disabled     |
| metallb                     | minikube | disabled     |
| metrics-server              | minikube | enabled ✅   |
| nvidia-driver-installer     | minikube | disabled     |
| nvidia-gpu-device-plugin    | minikube | disabled     |
| olm                         | minikube | disabled     |
| pod-security-policy         | minikube | disabled     |
| registry                    | minikube | disabled     |
| registry-aliases            | minikube | disabled     |
| registry-creds              | minikube | disabled     |
| storage-provisioner         | minikube | enabled ✅   |
| storage-provisioner-gluster | minikube | disabled     |
| volumesnapshots             | minikube | disabled     |
|-----------------------------|----------|--------------|
```

```
minikube dashboard
```

Press `Ctrl-C` to terminate the dashboard.

#### Installing Nginx to access dashboard on a headless VM

```
minikube service list
```

```
|----------------------|---------------------------|--------------|-----|
|      NAMESPACE       |           NAME            | TARGET PORT  | URL |
|----------------------|---------------------------|--------------|-----|
| default              | kubernetes                | No node port |
| kube-system          | kube-dns                  | No node port |
| kube-system          | metrics-server            | No node port |
| kubernetes-dashboard | dashboard-metrics-scraper | No node port |
| kubernetes-dashboard | kubernetes-dashboard      | No node port |
|----------------------|---------------------------|--------------|-----|
```

```
kubectl get svc -n kubernetes-dashboard
```

Container ports are not mapped to node ports.

```
NAME                        TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)    AGE
dashboard-metrics-scraper   ClusterIP   10.110.239.183   <none>        8000/TCP   3m35s
kubernetes-dashboard        ClusterIP   10.103.51.186    <none>        80/TCP     3m35s
```

```
kubectl edit svc kubernetes-dashboard -n kubernetes-dashboard
```

Change the line `type: ClusterIP` to `type: NodePort` to allow a custom Port. Add a new line `nodePort: 30000` after `targetPort: <port>`.

We will require both `minikube ip` and `nodePort: 30000` for **Nginx** configuration.

```
sudo apt install -y nginx
sudo vim /etc/nginx/sites-enabled/default
```

Change the `location` directive under `server` to the following:

```
location / {
    proxy_pass http://<minikube_ip>:<node_port>
}
```

Restart the web server with `sudo systemctl restart nginx`. Now open a web browser and navigate to your VM's public IP address.

### kubectl Commands

`kubectl create deployment --image <image> <instance_name>` - Deploys a container with `<instance_name>` using the `<image>`
`kubectl expose deployment <instance_name> --port=<port> --type=NodePort` - Exposes a port in the container with `<instance_name>`.
`kubectl get po` - Displays a list of pods
`kubectl get svc` - Displays a list of containers in the `default` namespace. Use `-A` option for all namespaces.

### Troubleshooting

1. If you get the following error on `sudo minikube start`:

```
Exiting due to GUEST_MISSING_CONNTRACK: Sorry, Kubernetes 1.20.2 requires conntrack to be installed in root's path
```

The following command should resolve the above issue:

```
sudo apt-get install -y conntrack
```

2. If you get the following error on `sudo minikube start --driver=docker`:

```
Exiting due to DRV_AS_ROOT: The "docker" driver should not be used with root privileges.
```

You should perform `minikube start --driver=docker` without `sudo` privilege.

3. If you get the following error on `minikube start --driver=docker`:

```
Exiting due to HOST_HOME_PERMISSION: Failed to save config: open /home/dennislwm/.minikube/profiles/minikube/config.json: permission denied
```

The following command should resolve the above issue:

```
sudo chown -R $USER $HOME/.minikube; chmod -R u+wrx $HOME/.minikube
```

4. If you get the following error on `sudo minikube start --driver=docker`:

```
Exiting due to GUEST_DRIVER_MISMATCH: The existing "minikube" cluster was created using the "none" driver, which is incompatible with requested "docker" driver.
```

Check if an existing profile exists using `sudo minikube profile list`. The following command should resolve the above issue:

```
sudo minikube delete --purge=true --all=true
```

### References

* [Docker Desktop WSL 2 backend](https://docs.docker.com/docker-for-windows/wsl/)
* [Install WSL on Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)
* [Ubuntu on WSL 2 Is Generally Available](https://ubuntu.com/blog/ubuntu-on-wsl-2-is-generally-available)
