# 石墨烯环境搭建 Ubuntu14.04

### 安装依赖

```bash
sudo apt-get install gcc-4.9 g++-4.9 cmake make libbz2-dev libdb++-dev libdb-dev libssl-dev openssl libreadline-dev autoconf libtool git
```

### 安装boost 1.57.0

```bash
BOOST_ROOT=$HOME/opt/boost_1_57_0
sudo apt-get update
sudo apt-get install autotools-dev build-essential g++ libbz2-dev libicu-dev python-dev
wget -c 'http://sourceforge.net/projects/boost/files/boost/1.57.0/boost_1_57_0.tar.bz2/download' -O boost_1_57_0.tar.bz2
[ $( sha256sum boost_1_57_0.tar.bz2 | cut -d ' ' -f 1 ) == "910c8c022a33ccec7f088bd65d4f14b466588dda94ba2124e78b8c57db264967" ] || ( echo 'Corrupt download' ; exit 1 )
tar xjf boost_1_57_0.tar.bz2
cd boost_1_57_0/
./bootstrap.sh "--prefix=$BOOST_ROOT"
./b2 install
```

### 安装石墨烯

```bash
cd ..
git clone https://github.com/cryptonomex/graphene.git
cd graphene
git submodule update --init --recursive
cmake -DBOOST_ROOT="$BOOST_ROOT" -DCMAKE_BUILD_TYPE=Debug .
make 
```



##### 开启验证节点

```bash
./programs/witness_node/witness_node
```

##### 打开rpc服务

修改witness_node_data_dir/config.ini文件中的rpc-endpoint

```bash
rpc-endpoint = 127.0.0.1:8090
```

##### 启动钱包

```bash
./programs/cli_wallet/cli_wallet
```

##### 导入账号

```
//设置密码
>>> set_password lb
//解锁账号
>>> unlock lb
// 解锁账号
>>> import_balance nathan [5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3] true
```

