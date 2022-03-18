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
