- 设置计算机名
```bash
# Master节点
hostnamectl set-hostname k8s-master

# Node1节点
hostnamectl set-hostname k8s-node1

# Node2节点
hostnamectl set-hostname k8s-node2
```
- 设置hosts
```bash
cat >> /etc/hosts << EOF
192.168.1.10 k8s-master
192.168.1.11 k8s-node1
192.168.1.12 k8s-node2
EOF
```
- 关闭防火墙
```
# 关闭防火墙
systemctl stop firewalld
systemctl disable firewalld

# 或者配置防火墙规则（生产环境推荐）
firewall-cmd --permanent --add-port=6443/tcp
firewall-cmd --permanent --add-port=2379-2380/tcp
firewall-cmd --permanent --add-port=10250-10255/tcp
firewall-cmd --reload
```
- 关闭 selinux
```bash
# 临时关闭
setenforce 0

# 永久关闭
sed -i 's/SELINUX=enforcing/SELINUX=disabled/' /etc/selinux/config

# 重启生效
reboot
```
-  配置Swap分区
```bash
# 临时关闭swap
swapoff -a

# 永久关闭（注释掉swap行）
sed -i '/ swap / s/^/#/' /etc/fstab
```
- 加载内核模块
```bash
cat >> /etc/modules-load.d/k8s.conf <> /etc/sysctl.d/k8s.conf << EOF
net.bridge.bridge-nf-call-iptables = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward = 1
EOF

# 应用配置
sysctl --system
```
- 安装container.io
```bash
dnf install containerd.io -y
systemctl enable --now containerd

containerd  config default > /etc/containerd/config.toml
sed -e 's/registry.k8s.io/k8s.nju.edu.cn/g' \
    -e 's/SystemdCgroup = false/SystemdCgroup = true/g' \
	-i.bak \
    /etc/containerd/config.toml

systemctl daemon-reload
systemctl restart  containerd
```
- 安装kubernetes
```bash
cat >> /etc/yum.repos.d/kubernetes.repo << EOF
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.tuna.tsinghua.edu.cn/kubernetes/core:/stable:/v1.34/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.34/rpm/repodata/repomd.xml.key
EOF

# 安装kubeadm、kubelet、kubectl
yum install -y kubeadm-1.34.0 kubelet-1.34.0 kubectl-1.34.0 --disableexcludes=kubernetes

# 启动kubelet
systemctl enable kubelet
systemctl start kubelet

# 验证安装
kubeadm version
kubelet --version
```
- 初始化集群
```bash
# 在Master节点执行
kubeadm init   --apiserver-advertise-address=192.168.1.10   --image-repository registry.aliyuncs.com/google_containers   --kubernetes-version v1.34.0   --service-cidr=10.96.0.0/12   --pod-network-cidr=10.244.0.0/16
```
- 设置kubectl环境
```bash
# 配置kubectl
mkdir -p $HOME/.kube
cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
chown $(id -u):$(id -g) $HOME/.kube/config

# 验证配置
kubectl get nodes
kubectl get pods -n kube-system
```
- 安装网络插件
```bash
# 安装Calico网络插件
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml

# 或者使用Helm安装
helm repo add projectcalico https://projectcalico.org/charts
helm install calico projectcalico/tigera-operator --version 3.28.0 -n calico-system --create-namespace

# 验证安装
kubectl get pods -n kube-system | grep calico
```
