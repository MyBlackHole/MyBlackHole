# install

```shell
cat <<EOF | sudo tee /etc/modules-load.d/k8s.conf
overlay
br_netfilter
EOF

sudo modprobe overlay
sudo modprobe br_netfilter

<!-- 设置所需的 sysctl 参数，参数在重新启动后保持不变 -->
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables  = 1
net.bridge.bridge-nf-call-ip6tables = 1
net.ipv4.ip_forward                 = 1
EOF

<!-- 应用 sysctl 参数而不重新启动 -->
sudo sysctl --system

<!-- 安装依赖 -->
sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    lsb-release


<!-- docker 安装 -->
<!-- 安装 docker gpg -->
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt update
sudo apt-get install docker-ce docker-ce-cli containerd.io

<!-- docker 配置 -->
sudo vim /etc/docker/daemon.json
{
  "exec-opts": ["native.cgroupdriver=systemd"],
  "registry-mirrors": ["https://98bnyivl.mirror.aliyuncs.com"]
}
<!-- 开机自启动 -->
sudo systemctl enable docker


<!-- kubernetes -->
wget https://mirrors.aliyun.com/kubernetes/apt/doc/apt-key.gpg
sudo apt-key add apt-key.gpg
add-apt-repository "deb https://mirrors.aliyun.com/kubernetes/apt/ kubernetes-xenial main"
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
<!-- 锁定版本 -->
sudo apt-mark hold kubelet kubeadm kubectl



kubeadm init \
--apiserver-advertise-address=192.168.56.1  \
--ignore-preflight-errors=all \
--image-repository registry.aliyuncs.com/google_containers \
--service-cidr=10.1.0.0/16 \
--pod-network-cidr=10.244.0.0/16


<!-- 集群连接配置 -->
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

<!-- 添加节点到集群 -->
kubeadm join 192.168.56.1:6443 --token arj03m.b6ia8p9f3grzdpqn \
	--discovery-token-ca-cert-hash sha256:33c4fbbf00a0ed0805ebcc95bc7b8dd964411c06c5e68fb80e3013c392299325

<!-- kubectl 需要调用 -->
<!-- sudo chmod 666 /etc/kubernetes/admin.conf -->

# 安装网络cni
kubectl apply -f kube-flannel.yml

# 本地8080 容器80 对接 测试(ClusterIP模式)
kubectl port-forward nginx-deployment-6f7d8d4d55-qkvw4 8080:80
```



```shell
cat <<EOF | sudo tee /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/
enabled=1
gpgcheck=1
gpgkey=https://pkgs.k8s.io/core:/stable:/v1.28/rpm/repodata/repomd.xml.key
exclude=kubelet kubeadm kubectl cri-tools kubernetes-cni
EOF

sudo yum install -y kubelet kubeadm kubectl --disableexcludes=kubernetes
sudo systemctl enable --now kubelet

ctr images import pause.img
......

kubeadm init \
      --pod-network-cidr 10.244.0.0/16 \
      --cri-socket /run/cri-containerd/cri-containerd.sock \
      --v 5 \
      --ignore-preflight-errors=all
```
