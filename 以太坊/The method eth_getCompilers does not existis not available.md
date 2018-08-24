# The method eth_getCompilers does not exist/is not available

### 原因

eth_complile*方法已经从eth API中移除。

因为以太坊是处理以太坊虚拟机中的二进制代码，不应该处理高级语言相关的功能。

[官方解释](https://github.com/ethereum/EIPs/issues/209)

### 解决方法

##### 在线编译

[在线编译网址](http://remix.ethereum.org)

[源码](https://github.com/ethereum/remix-ide)

##### 本地安装solc环境

```bash
sudo add-apt-repository ppa:ethereum/ethereum
sudo apt-get update
sudo apt-get install solc
which solc
```

##### truffle使用（强烈推荐）

[官网](https://truffleframework.com/docs)

