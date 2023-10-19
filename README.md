# Kubernetes Cluster Setup On Ubuntu 22.04
A Kubernetes cluster is used to manage and orchestrate containerized applications at scale. Kubernetes is an open-source container orchestration platform that automates the deployment, scaling, and management of containerized applications.

## Let's delve into the process of establishing a Kubernetes cluster.
<br>
1 .  When you run sudo -s and provide the necessary authentication (usually your password), you enter a new shell environment where you have administrative control over the system.


```
sudo -s
```
<br>
<br>

2 . when you run this command, it appends these two hostname-to-IP mappings to your /etc/hosts file, allowing your system to resolve the hostnames k8s-control and k8s-2 to their respective IP addresses when you use those hostnames in network-related operations.
```
printf "\n192.168.15.93 k8s-control\n192.168.15.94 k8s-2\n\n" >> /etc/hosts
```
<br>
<br>
3 .  The command is modifying the containerd configuration to ensure that the "overlay" and "br_netfilter" kernel modules are loaded during the system's startup. 

```
printf "overlay\nbr_netfilter\n" >> /etc/modules-load.d/containerd.conf
```
<br>
<br>
4 . This command is used to load the kernel module named "overlay" and "br_netfilter" respectively into the running kernel.

```
modprobe overlay
modprobe br_netfilter
```
<br>
<br>
5 . This command  configure the kernel to support networking requirements and packet filtering specific to containerization and orchestration environments.

```
printf "net.bridge.bridge-nf-call-iptables = 1\nnet.ipv4.ip_forward = 1\nnet.bridge.bridge-nf-call-ip6tables = 1\n" >> /etc/sysctl.d/99-kubernetes-cri.conf
```
<br>
<br>
6 . When you run sysctl --system, it reads these configuration files and applies the specified parameter settings, ensuring that the kernel operates with the desired configuration.

```
sysctl --system
```
<br>
<br>
7 . The commands  provided are used to download and install the containerd runtime on a Linux system.

```
wget https://github.com/containerd/containerd/releases/download/v1.6.16/containerd-1.6.16-linux-amd64.tar.gz -P /tmp/
tar Cxzvf /usr/local /tmp/containerd-1.6.16-linux-amd64.tar.gz
```
<br>
<br>
8 . The commands are setting up and configuring the systemd service for containerd.

```
wget https://raw.githubusercontent.com/containerd/containerd/main/containerd.service -P /etc/systemd/system/
systemctl daemon-reload
systemctl enable --now containerd
```
<br>
<br>
9 . The provided commands are used to download the runc binary and install it on a Linux system.

```
wget https://github.com/opencontainers/runc/releases/download/v1.1.4/runc.amd64 -P /tmp/
install -m 755 /tmp/runc.amd64 /usr/local/sbin/runc
```
<br>
<br>
10 . After running these commands, the CNI plugins are extracted from the archive and installed in the /opt/cni/bin directory. These CNI plugins are used for container networking in containerization platforms like Kubernetes and allow you to manage container network configurations.

```
wget https://github.com/containernetworking/plugins/releases/download/v1.2.0/cni-plugins-linux-amd64-v1.2.0.tgz -P /tmp/
mkdir -p /opt/cni/bin
tar Cxzvf /opt/cni/bin /tmp/cni-plugins-linux-amd64-v1.2.0.tgz
```
<br>
<br>
11 . After executing  these commands , containerd should be running with the updated configuration that enables systemd cgroup management. 

```
mkdir -p /etc/containerd
containerd config default | tee /etc/containerd/config.toml   <<<<<<<<<<< manually edit and change systemdCgroup to true
systemctl restart containerd
```
<br>
<br>
12 . not clear

```
swapoff -a  <<<<<<<< just disable it in /etc/fstab instead
```
<br>
<br>
13 .  Installs and updates necessary packages.

```
apt-get update
apt-get install -y apt-transport-https ca-certificates curl
```



 
