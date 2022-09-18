Kubernetes
--
- 设置镜像源
```bash
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://mirrors.aliyun.com/kubernetes/yum/repos/kubernetes-el7-x86_64/
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://mirrors.aliyun.com/kubernetes/yum/doc/yum-key.gpg https://mirrors.aliyun.com/kubernetes/yum/doc/rpm-package-key.gpg
EOF
```
- 安装
```bash
dnf install -y kubelet kubeadm kubectl
```
- 如果安装失败，尝试这个
```bash
dnf install -y --nogpgcheck kubelet kubeadm kubectl
```
- 启动
```bash
systemctl enable kubelet && systemctl start kubelet
```
