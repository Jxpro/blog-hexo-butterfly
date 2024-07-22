---
title: Ubuntu 搭建 k8s 集群 ( 内网，快速纯命令版 )
description: 记录内网环境下 Ubuntu 搭建 k8s 集群的过程，主要内容只包含用于复制并直接粘贴运行的命令操作，不包含过多的解释和说明，适合快速搭建测试环境集群。
date: 2024/07/07 11:58:04
updated: 2024/07/11 22:23:44
categories:
  - Tool
  - Kubernetes
tags:
  - Kubernetes
---

## 软件版本

|    名称    |                      版本                       |
| :--------: | :---------------------------------------------: |
|   System   | Ubuntu 22.04 LTS (GNU/Linux 5.15.0-107-generic) |
| Kubernetes |                      1.30                       |
|   Docker   |                     27.0.1                      |

## 系统设置

>   因为默认满足要求或已前置，以下省略了：
>
>   -   SELinux （Ubuntu 没有）
>   -   主机名设置（hostnamectl 设置）
>   -   防火墙详细设置（ufw 设置）
>   -   虚拟内存（free -m 查看）
>   -   时钟同步（timedatectl 设置）
>   -   网络配置（sysctl 查看）
>   -   IPVS（可选）

### DNS

修改 `hosts` 文件，将IP地址和主机名填进去

```shell
sudo tee -a /etc/hosts <<-'EOF'
# k8s cluster
172.29.140.34 master
172.29.140.35 node1
172.29.140.36 node2
EOF
```

### 内核模块

```shell
sudo tee /etc/modules-load.d/k8s.conf <<-'EOF'
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter
```

### 防火墙

云服务需要在安全组中操作防火墙，建议先开启所有端口，测试通过后逐一关闭

### IPVS (可选)

```shell
sudo apt update && sudo apt install -y ipset ipvsadm
sudo tee /etc/modules-load.d/ipvs.conf <<-'EOF'
ip_vs
ip_vs_rr
ip_vs_wrr
ip_vs_sh
nf_conntrack
EOF
```

## 软件安装

### Docker

```shell
for pkg in docker.io docker-doc docker-compose docker-compose-v2 podman-docker containerd runc; do sudo apt-get remove $pkg; done

# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

# Add the repository to Apt sources:
echo   "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] https://mirrors.cloud.tencent.com/docker-ce/linux/ubuntu/ \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" |   sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update

# Install using the apt repository
sudo apt-get install -y docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Configure the registry mirrors
sudo mkdir -p /etc/docker
sudo tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://mirrors.docker.com"]
}
EOF
sudo systemctl daemon-reload
sudo systemctl restart docker
```

### Containerd

```shell
sudo containerd config default | sudo tee /etc/containerd/config.toml > /dev/null
sudo sed -i '/registry.k8s.io\/pause/s|3.8|3.9|' /etc/containerd/config.toml
sudo sed -i 's/SystemdCgroup\ =\ false/SystemdCgroup\ =\ true/g' /etc/containerd/config.toml
sudo sed -i 's/config_path\ =\ ""/config_path\ =\ "\/etc\/containerd\/certs.d"/g' /etc/containerd/config.toml
sudo systemctl restart containerd
```

```shell
sudo mkdir -p /etc/containerd/certs.d/docker.io
sudo tee /etc/containerd/certs.d/docker.io/hosts.toml <<-'EOF'
server = "https://docker.io"

[host."https://mirrors.docker.com"]
  capabilities = ["pull", "resolve"]
EOF

sudo mkdir -p /etc/containerd/certs.d/registry-1.docker.io
sudo tee /etc/containerd/certs.d/registry-1.docker.io/hosts.toml <<-'EOF'
server = "https://registry-1.docker.io"

[host."https://mirrors.docker.com"]
  capabilities = ["pull", "resolve"]
EOF

sudo mkdir -p /etc/containerd/certs.d/registry.k8s.io
sudo tee /etc/containerd/certs.d/registry.k8s.io/hosts.toml <<-'EOF'
server = "https://registry.k8s.io"

[host."https://mirrors.k8s.com"]
  capabilities = ["pull", "resolve"]
EOF

sudo mkdir -p /etc/containerd/certs.d/quay.io
sudo tee /etc/containerd/certs.d/quay.io/hosts.toml << 'EOF'
server = "https://quay.io"

[host."https://mirrors.quay.com"]
  capabilities = ["pull", "resolve"]
EOF
```

### Kubernetes

```shell
sudo apt update && sudo apt install -y apt-transport-https

curl -fsSL https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.30/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.30/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

tee -a .zshrc <<-'EOF'
# k8s completion
source <(kubectl completion zsh)
source <(kubeadm completion zsh)
EOF
```

## 配置 Master

### 拉取镜像

```shell
sudo kubeadm config images pull --kubernetes-version=$(kubeadm version -o short)
```

### 初始化

```shell
sudo kubeadm init \
      --apiserver-advertise-address=172.29.140.34 \
      --pod-network-cidr=192.168.0.0/16 \
      --service-cidr=10.96.0.0/16 \
      --kubernetes-version=$(kubeadm version -o short)
```

### 命令行工具

```shell
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

## 添加 Worker

### 加入集群

```shell
sudo kubeadm join ip:port --token xxxxxx \
        --discovery-token-ca-cert-hash sha256:xxxxxxxxxxxx
```

### 查看节点

```shell
kubectl get nodes
NAME         STATUS     ROLES           AGE     VERSION
k8s-master   NotReady   control-plane   4m14s   v1.30.2
k8s-node1    NotReady   <none>          25s     v1.30.2
```

## 网络插件 Calico

>   在 `Master` 节点上操作，直接看 `6.4` 节的解决方案

### 创建 tigera-operator

```shell
wget https://raw.staticdn.net/projectcalico/calico/v3.28.0/manifests/tigera-operator.yaml
kubectl create -f tigera-operator.yaml
```

### 启动 calico

下载或打开配置文件

```shell
wget https://raw.staticdn.net/projectcalico/calico/v3.28.0/manifests/custom-resources.yaml
kubectl apply -f custom-resources.yaml
```

### 遇到的问题

相比于公网 Pod 无法启动，内网集群 Pod 虽然可以启动，但是仍然无法通信，目前没找到解决办法

```shell
Readiness probe failed: calico/node is not ready: BIRD is not ready: Error querying BIRD: unable to connect to BIRDv4 socket: dial unix /var/run/calico/bird.ctl: connect: connection refused
```

### 解决方案

>   上面的命令都不用管了

使用 `v3.25` 版本的另一种部署方式可以解决问题

```shell
kubectl apply -f https://calico-v3-25.netlify.app/archive/v3.25/manifests/calico.yaml
