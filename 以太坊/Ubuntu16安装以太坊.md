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

5. geth常用参数：
   - --rpc 开启HTTP-RPC服务
   - -–rpcaddr 指定HTTP-RPC服务的地址，默认是localhost
   - -–port 网络监听的端口，默认为8545
   - -–rpccorsdomain 逗号分隔的域列表，指定HTTP-RPC服务允许从哪些域过来的跨域请求，*接受表示所有的域
   - -–rpcapi 设定开放给HTTP-RPC的接口，默认只开放eth、net、web3
   - --ws 启用WebSockets-RPC服务
   - –-wsaddr 指定WebSockets-RPC服务地址，默认值localhost
   - -–wsport 指定WebSockets-RPC服务端口，默认值8546
   - -–wsapi 通过WebSockets-RPC提供的API，默认eth, net, web3
   - -–wsorigins 指定WebSockets-RPC服务允许从哪些域过来的跨域请求，*表示接受表示所有的域
   - -–datadir 设置当前区块链网络数据存放的位置
   - -–identity 区块链的标识，用于标识目前网络的名字
   - -–networkid 设置当前区块链的网络 ID，用于区分不同的网络，默认是1
   - -–nodiscover 禁止网络中的对等节点发现你的节点。如果打算在本地网络中与其他人一起使用该私有区块链，就请不要使用此参数。 –dev console 开启一个可交互的JavaScript Console
   - -–ipcdisable 禁用IPC-RPC服务
   - -–ipcapi 通过IPC-RPC接口提供的API，默认值admin, debug, eth, miner, net, personal, shh, txpool, web3
   - -–ipcpath 指定IPC路径

### 私链搭建

1. 定义创世状态, 修改nonce防止和其他节点链接到你

   e.g. genesis.json

   ``` json
   {
     "config": {
           "chainId": 11,
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

6. 启动一个节点

   ```bash
   geth --datadir /e/eth/a init ./genesis.json
   geth --datadir ./datadir/a --networkid 22 --nodiscover --port 30303 --c --rpcport 8545 --rpcaddr 0.0.0.0 --ipcdisable console
   ```

7. 启动第二个节点

   ```bash
   geth --datadir /e/eth/b init ./genesis.json //同一个区块链上的genesis.json必须一样
    geth --datadir ./datadir/a --networkid 23 --nodiscover --port 30303 --rpc --rpccorsdomain "*" --rpcport 8545 --rpcaddr 0.0.0.0 --ipcdisable console
   ```

8. 连接两个节点

   ```bash
   //从其中一个节点中获得enode
   >admin.nodeInfo.enode
   "enode://a690738d5a7a079bcf4b38371f28d168317c9da9652c41c14ec123d6f4f1b13d3153b72fbe158ad39fc92d95403c5e17b510d71dfdf47a6a3aad7a182c1cd999@[::]:30303?discport=0"
   //在另一个节点环境添加peer
   admin.addPeer("enode://a690738d5a7a079bcf4b38371f28d168317c9da9652c41c14ec123d6f4f1b13d3153b72fbe158ad39fc92d95403c5e17b510d71dfdf47a6a3aad7a182c1cd999@[::]:30303?discport=0")
   ```


