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
- 关闭交换内存
```bash
swapoff -a
sed -i 's/.*swap.*/#&/' /etc/fstab
```
- 关闭 selinux
```bash
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
```
- 关闭防火墙
```bash
systemctl disable --now firewalld
```
- 设置kuberntes
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter
```
- 设置内核转发
```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

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

dnf install -y kubelet kubeadm kubectl
systemctl enable --now kubelet

crictl config --set runtime-endpoint=unix:///run/containerd/containerd.sock
```
- 初始化集群
```bash
kubeadm init --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers \
--apiserver-advertise-address=192.168.0.170 \
--kubernetes-version=v1.28.8 \
--service-cidr=192.16.0.0/16 \
--pod-network-cidr=10.0.0.0/16
```
- 设置kubectl环境
```bash
cat <<EOF >> /root/.bashrc
export KUBECONFIG=/etc/kubernetes/admin.conf
EOF

source /root/.bashrc
```
- 安装网络插件
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```
