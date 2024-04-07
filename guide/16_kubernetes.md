```bash
hostnamectl set-hostname  k8s-master-01
```
```bash
echo "192.168.0.170   k8s-master-01"  >>/etc/hosts
```
```bash
swapoff -a
sed -i 's/.*swap.*/#&/' /etc/fstab
```
```bash
setenforce 0
sed -i 's/^SELINUX=enforcing$/SELINUX=disabled/' /etc/selinux/config
```
```bash
systemctl disable --now firewalld
```
```bash
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

modprobe overlay
modprobe br_netfilter
```
```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

sysctl --system
```
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
```bash
kubeadm init --image-repository=registry.cn-hangzhou.aliyuncs.com/google_containers \
--apiserver-advertise-address=10.1.1.147 \
--kubernetes-version=v1.28.8 \
--service-cidr=192.16.0.0/16 \
--pod-network-cidr=10.0.0.0/16
```
```bash
cat <<EOF >> /root/.bashrc
export KUBECONFIG=/etc/kubernetes/admin.conf
EOF

source /root/.bashrc
```
```bash
kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```