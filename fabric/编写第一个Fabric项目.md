# 编写第一个Fabric项目

### 删除所有的docker

```bash
docker rm -f $(docker ps -aq)
```

### 清除docker使用的网络

```bash
docker network prune
```

### 清除原有链码

```bash
docker rmi dev-peer0.org1.example.com-fabcar-1.0-5c906e402ed29f20260ae42283216aa75549c571e2e380f3615826365d8269ba
```

### 安装fabric-ca-client, fabric-client

node： 出错时，检查npm的版本是否是5.6.0，node版本8.x.x

```bash
npm install
```

### 启动fabric

```bash
./startFabric.sh
```

### 注册admin账号

```bash
$ node enrollAdmin.js
//在当前目录下会创建一个新的目录，hfc-key-store
$ tree hfc-key-store
hfc-key-store/
├── 7530a4844556c1379d65930664fd3b20e108c934dc0e9cc27d3fc081512c05da-priv
├── 7530a4844556c1379d65930664fd3b20e108c934dc0e9cc27d3fc081512c05da-pub
└── admin
```

### 注册用户账号

```bash
node registerUser.js
```

### 检查 user1

```bash
$ fabric_client.getUserContext('user1', true);
//如果出现以下问题，不要紧，忽略就好
-bash: 未预期的符号 `'user1',' 附近有语法错误
```

### 查询账本信息

```bash
$ node query.js
Successfully loaded user1 from persistence
Query has completed, checking results
Response is  [{"Key":"CAR0", "Record":{"colour":"blue","make":"Toyota","model":"Prius","owner":"Tomoko"}},
{"Key":"CAR1",   "Record":{"colour":"red","make":"Ford","model":"Mustang","owner":"Brad"}},
{"Key":"CAR2", "Record":{"colour":"green","make":"Hyundai","model":"Tucson","owner":"Jin Soo"}},
{"Key":"CAR3", "Record":{"colour":"yellow","make":"Volkswagen","model":"Passat","owner":"Max"}},
{"Key":"CAR4", "Record":{"colour":"black","make":"Tesla","model":"S","owner":"Adriana"}},
{"Key":"CAR5", "Record":{"colour":"purple","make":"Peugeot","model":"205","owner":"Michel"}},
{"Key":"CAR6", "Record":{"colour":"white","make":"Chery","model":"S22L","owner":"Aarav"}},
{"Key":"CAR7", "Record":{"colour":"violet","make":"Fiat","model":"Punto","owner":"Pari"}},
{"Key":"CAR8", "Record":{"colour":"indigo","make":"Tata","model":"Nano","owner":"Valeria"}},
{"Key":"CAR9", "Record":{"colour":"brown","make":"Holden","model":"Barina","owner":"Shotaro"}}]
```

### 更新账本信息

##### 修改invoke.js中对应的代码为

```javascript
var request = {
  //targets: let default to the peer assigned to the client
  chaincodeId: 'fabcar',
  fcn: 'createCar',
  args: ['CAR10', 'Chevy', 'Volt', 'Red', 'Nick'],
  chainId: 'mychannel',
  txId: tx_id
};
```

##### 执行代码

```bash
$ node invoke.js
Successfully committed the change to the ledger by the peer
//如果出现下面的错误，修改当前目录下package.json中的fabric-client和fabric-ca-client版本号为^1.2.1,然后执行node install
Failed to invoke successfully :: TypeError: fabric_client.newEventHub is not a function
```

