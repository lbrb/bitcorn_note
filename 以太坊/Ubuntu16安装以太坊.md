# Ubuntu16搭建以太坊私有链环境

### 安装go

1. [下载go](https://dl.google.com/go/go1.10.3.linux-amd64.tar.gz)

2. 解压

   ``` bash
    tar -C /usr/local -xzf go1.10.3.linux-amd64.tar.gz
   ```

3. 添加环境变量

   ``` bash
   vim /etc/profile
   export PATH=$PATH:/usr/local/go/bin
   source /etc/profile
   ```


### 安装eth

1. 下载

   ``` bash
   git clone https://github.com/ethereum/go-ethereum
   ```

2. make

   ```bash
   cd go-ethereum
   make geth //只编译geth
   make all //编译所有工具
   ```

3. 查看geth是否安装成功

   ``` bash
   ./build/bin/geth version
   ```

4. start node

   ```bash
   ./build/bin/geth
   ```

### 私链搭建

1. 定义创世状态, 修改nonce防止和其他节点链接到你

   e.g. genesis.json

   ``` json
   {
     "config": {
           "chainId": 0,
           "homesteadBlock": 0,
           "eip155Block": 0,
           "eip158Block": 0
       },
     "alloc"      : {},
     "coinbase"   : "0x0000000000000000000000000000000000000000",
     "difficulty" : "0x20000",
     "extraData"  : "",
     "gasLimit"   : "0x2fefd8",
     "nonce"      : "0x0000000000000042",
     "mixhash"    : "0x0000000000000000000000000000000000000000000000000000000000000000",
     "parentHash" : "0x0000000000000000000000000000000000000000000000000000000000000000",
     "timestamp"  : "0x00"
   }
   ```

   定义初始账号

   ``` json
   "alloc": {
     "0x0000000000000000000000000000000000000001": {"balance": "111111111"},
     "0x0000000000000000000000000000000000000002": {"balance": "222222222"}
   }
   ```

2. 删除已缓存的数据

   ``` bash
   geth removedb
   ```

3. 重新初始化每个节点

   ``` bash
   geth init path/to/genesis.json
   ```

4. 启动bootnode

   ```bash
   bootnode --genkey=boot.key
   bootnode --nodekey=boot.key
   ```

5. 启动成员节点

   ```bash
   geth --datadir=datadir --bootnodes=enode://66abf9b6ff7cbbd6bde7312752dabd43cbaccd75a6af4e2560bc210817d032fd333189e4eb95b69a4beda2c304626d9509ef01662780c162c6ea5da7a22637c8@192.168.235.130:30301
   ```

