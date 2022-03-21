# datacenter
资料存放
所需的11个依赖如下：

audit-libs-python-2.8.5-4.el7.x86_64.rpm
checkpolicy-2.5-8.el7.x86_64.rpm
libcgroup-0.41-21.el7.x86_64.rpm
libseccomp-2.3.1-3.el7.x86_64.rpm
libsemanage-python-2.5-14.el7.x86_64.rpm
policycoreutils-python-2.5-33.el7.x86_64.rpm
python-IPy-0.75-6.el7.noarch.rpm
setools-libs-3.3.8-4.el7.x86_64.rpm
pigz-2.3.3-1.el7.centos.x86_64.rpm
libtool-ltdl-2.4.2-22.el7_3.x86_64.rpm
container-selinux-2.9-4.el7.noarch.rpm
安装
1. 上传rpm 包 和 11 个依赖到自己服务目录下
2. 批量安装依赖包
（在上传文件下）

rpm -Uvh *.rpm --nodeps --force
3. 安装 container-selinux-2.9-4.el7.noarch.rpm
rpm -Uvh container-selinux-2.9-4.el7.noarch.rpm
4. 安装 docker
rpm -Uvh docker-ce-18.03.1.ce-1.el7.centos.x86_64.rpm 
5. 启动docker
systemctl start docker
6. 设置docker 自启动
systemctl enable docker
离线下载镜像
1. docker save : 将指定镜像保存成 tar 归档文件
docker save [options] image [image……]

options

-o :输出到的文件。

示例：

docker save -o minio.tar minio
2. scp 远程复制（迁移到另外一台服务器）
scp -r minio.tar root@192.168.2.207:/home/software
3. 导入镜像
docker load -i minio.tar

docker run -d --name=minio -p 9000:9000 -v /mnt/data:/data --restart=always -e "MINIO_ROOT_USER=root" -e "MINIO_ROOT_PASSWORD=xxxxxx" minio/minio server /data

docker run -itd --name redis -p 6379:6379 --restart=always redis --requirepass "xxxxx2021"
# 二 安装docker-compose

下载：

 curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
1
修改权限：

chmod +x /usr/local/bin/docker-compose

查看是否安装成功：

docker-compose -version

curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose

参考 https://blog.csdn.net/qq_38414907/article/details/122607403


# 在线安装 docker
一、安装docker-ce
从镜像仓库安装：

更新yum包：

sudo yum update

设置镜像仓库：
sudo yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo

更新yum索引：

sudo yum makecache fast

安装最新docker-ce:

sudo yum install docker-ce

启动/停止 docker：

sudo systemctl start docker 启动
sudo systemctl restart docker 重启
sudo systemctl enable docker 加入开机启动

卸载docker：

sudo yum remove docker-ce

二、安装docker-compose
github上下载太慢，使用国内镜像

下载：

 curl -L https://get.daocloud.io/docker/compose/releases/download/1.25.4/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose
1
修改权限：

chmod +x /usr/local/bin/docker-compose

查看是否安装成功：

docker-compose -version

原文链接：https://blog.csdn.net/qq_38414907/article/details/122607403

# K8S+DOCKER
#修改主机名
vim /etc/hostname 
reboot
history
#关闭防火墙
systemctl disable firewalld
systemctl status firewalld
systemctl stop firewalld
systemctl status firewalld
dnf journalctl -xe
#修改仓库地址
sed -e 's|^mirrorlist=|#mirrorlist=|g'     -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.aliyun.com/rockylinux|g'     -i.bak     /etc/yum.repos.d/Rocky-*.repo
#重置缓存
dnf makecache
dnf install libseccomp-devel.x86_64 -y
#go安装
dnf install go
go version
#runc安装
mkdir /opt/opencontainers
cd /opt/opencontainers
git clone https://github.com/opencontainers/runc
cd runc/
make
dnf -y install gcc gcc-c++ automake autoconf libtool make
runc -v
#containerd安装
wget -P /opt/downloads https://download.fastgit.org/containerd/containerd/releases/download/v1.5.8/cri-containerd-cni-1.5.8-linux-amd64.tar.gz
tar -tf /opt/downloads/cri-containerd-cni-1.5.8-linux-amd64.tar.gz
cd /
tar -C / -xzf /opt/downloads/cri-containerd-cni-1.5.8-linux-amd64.tar.gz
vim ~/.bashrc
source ~/.bashrc
mkdir /etc/containerd
containerd config default > /etc/containerd/config.toml
systemctl enable containerd
systemctl restart containerd
systemctl status containerd
journalctl -xe
ctr version
ctr namespace ls
wget -P /opt/downloads https://github.com/moby/buildkit/releases/download/v0.9.3/buildkit-v0.9.3.linux-amd64.tar.gz
nerdctl build
wget https://github.com/moby/buildkit/releases/download/v0.9.2/buildkit-v0.9.2.linux-amd64.tar.gz
reboot
wget -P /opt/downloads https://github.com/moby/buildkit/releases/download/v0.9.3/buildkit-v0.9.3.linux-amd64.tar.gz
cd /opt/downloads/
date
dnf install -y chrony
systemctl enable --now chronyd
rm -rf /etc/localtime
ln -s /usr/share/zoneinfo/Asia/Shanghai  /etc/localtime
date
mkdir /etc/buildkit/
dnf install -y nfs-utils rpcbind
rpm -qa nfs-utils rpcbind
mkdir -p /k8s-volume;chmod -R 755 /k8s-volume
lsblk
echo “vm.swappiness = 0”>> /etc/sysctl.conf 
swapoff -a && swapon -a
sysctl -p
wget -c https://sealyun.oss-cn-beijing.aliyuncs.com/latest/sealos && chmod +x sealos && mv sealos /usr/bin
cd /root/
wget -c https://sealyun.oss-cn-beijing.aliyuncs.com/05a3db657821277f5f3b92d834bbaf98-v1.22.0/kube1.22.0.tar.gz
sealos init --master 192.168.1.32 --master 192.168.1.35 --user root  --passwd '123456' --pkg-url /root/kube1.22.0.tar.gz --version v1.22.0
kubectl get pod -A
kubectl get nodes
clear
kubectl taint nodes --all node-role.kubernetes.io/master-
#此处一共修改两个地方：
	1.- --feature-gates=TTLAfterFinished=true,RemoveSelfLink=false       //动态PV关键属性
	2.添加：- --service-node-port-range=1-65535			     //监听应用端口
vim /etc/kubernetes/manifests/kube-apiserver.yaml




systemctl daemon-reload
systemctl restart kubelet
systemctl status kubelet
kubectl get nodes
dnf install -y nfs-utils rpcbind
chmod -R 755 /k8s-volume
ifconfig
vim /etc/exports                             //此处修改为自己的nfs服务器
systemctl restart rpcbind
systemctl restart nfs-server
systemctl enable rpcbind
systemctl enable nfs-server
exportfs -v
ctr i pull quay.io/external_storage/nfs-client-provisioner:v3.1.0-k8s1.11
vim /opt/nfs-client-provisioner/deploy.yaml 
kubectl apply -f /opt/nfs-client-provisioner/deploy.yaml 
showmount -e
kubectl apply -f /opt/nfs-client-provisioner/deploy.yaml 
kubectl get pod -A 
kubectl describe pod -n nfs nfs-client-provisioner-b5d7d5fc9-h9nkh
kubectl get pod -A 
ctr i pull docker.io/library/mysql:8.0.27
vim /opt/mysql/deploy.yaml 
kubectl apply -f /opt/mysql/deploy.yaml 
kubectl get pod -A 
kubectl describe pod -n mysql mysql-0
kubectl describe pods
kubectl get pv
kubectl get pod -A
kubectl exec -it -n mysql mysql-0 -- bash
kubectl get pod -A
kubectl describe pod -n  mysql mysql-0
ctr i pull docker.io/library/redis:6.2.5-buster
kubectl apply -f /opt/redis/deploy.yaml 
kubectl get pod -A
kubectl apply -f /opt/redis/deploy.yaml 
kubectl get pod -A
kubectl exec -it -n mysql mysql-0 -- bash
history
#nacos部署前，请先部署MySQL
#nacos deploy.yaml 数据源需修改为自己的数据源
ctr i pull docker.io/nacos/nacos-server:2.0.3-slim
ctr i pull docker.io/nacos/nacos-peer-finder-plugin:1.1
kubectl apply -f /opt/nacos/deploy3.yaml
kubectl get pod --all-namespaces

