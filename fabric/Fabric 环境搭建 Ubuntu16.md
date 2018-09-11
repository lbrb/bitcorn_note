# Fabric 环境搭建 Ubuntu16

### 安装GO语言环境

下载

```bash
curl -O https://dl.google.com/go/go1.11.linux-amd64.tar.gz
```

下载完成后，解压目录，并移动到合适的位置（推荐/usr/local)

```bash
tar -xvf 
sudo mv go /usr/local
```

配置环境变量

```bash
vim /etc/profile
export GOPATH =/home/liangbin/Documents/Go
export PATH=$PATH:/usr/local/go/bin:$GOPATH/bin
```

验证是否安装成功

```bash
$ go version
go version go1.11 linux/amd64
```

### 安装nodejs和npm

```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs

npm install npm@5.6.0 -g
```



### 安装依赖包

```bash
sudo apt-get update
sudo apt-get install -y libsnappy-dev zlib1g-dev libbz2-dev libltdl-dev libtool
```

### 安装docker

```bash
curl -faSL https://get.docker.com/ | sh
将账号添加到docker用户组中
sudo usermod -aG docker liangbin
```

### 获取代码

```bash
mkdir -p $GOPATH/src/github.com/hyperledger
cd $GOPATH/src/github.com/hyperledger
//下载fabric
git clone --single-branch -b master --depth 1  http://gerrit.hyperledger.org/r/fabric
//下载fabric-ca
git clone --single-branch -b master --depth 1  http://gerrit.hyperledger.org/r/fabric-ca
```

### 编译安装fabric-peer组件

```bash
cd fabric
make peer
```

### 编译安装fabric-orderer组件

```bash
make orderer
```

### 编译安装fabric-ca组件

```bash
go install  -ldflags " -linkmode external -extldflags '-static -lpthread'" github.com/hyperledger/fabric-ca/cmd/..
```

