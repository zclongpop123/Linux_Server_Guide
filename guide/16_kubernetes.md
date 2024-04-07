- 设置计算机名
```bash
hostnamectl set-hostname  k8s-master-01
```
- 设置hosts
```bash
echo "192.168.0.170   k8s-master-01"  >>/etc/hosts
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
cat <<EOF | tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.28/rpm/
enabled=1
gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes-new/core/stable/v1.28/rpm/repodata/repomd.xml.key
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
