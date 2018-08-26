# Truffle

### 安装Nodejs

### 安装Turffle

```bash
npm install -g truffle
```

### 快速开始

##### 创建一个项目

1. 创建一个目录

   ```bash
   mkdir MetaCoin
   cd MetaCoin
   ```

2. 下载box

   ```bash
   truffle unbox metacoin // 可以使用truffle unbox 下载其他box
   ```

3. truffle 项目结构

   1. contracts/: 智能合约的目录
   2. migrations/: 发布脚本的目录

   3. test/ : 测试文件的目录

   4. truffle.js: 配置文件

      1. 配置network

         ```bash
         module.exports = {
           // See <http://truffleframework.com/docs/advanced/configuration>
           // to customize your Truffle configuration!
           networks:{
         	  development:{
         		  host:"localhost",
         		  port:7545,
         		  network_id:"5777"
         	  }
           }
         };
         ```

4. 编译合约

   ```bash
   >truffle compile
   Compiling .\contracts\ConvertLib.sol...
   Compiling .\contracts\MetaCoin.sol...
   Compiling .\contracts\Migrations.sol...
   Writing artifacts to .\build\contracts
   
   ```

5. 依赖

   1. 依赖文件

      ```bash
      import "./AnotherContract.sol";
      ```

   2. 依赖包名

      ```bash
      import "somepackage/SomeContract.slo";
      ```


6. 执行migrate

   ```bash
   >truffle migrate
   Using network 'development'.
   
   Running migration: 1_initial_migration.js
     Deploying Migrations...
     ... 0xf2cc4c83c0a000182eef73490d786a021759ff5fc0e834beec8ba1f246b27e72
     Migrations: 0x524a68c5becef22c8af11e1e4f1cbafdf89a3459
   Saving successful migration to network...
     ... 0x6c199abce5a4b7d3c4beae28618ce238ad49f135bc451d51a359647014d29c64
   Saving artifacts...
   Running migration: 2_deploy_contracts.js
     Deploying ConvertLib...
     ... 0x7f7ea181c612b5de4b343d407255385cd7fc5e98a86e18c3a1cdb125f711f78e
     ConvertLib: 0x056a1adcea2c3687af8e1c2674c0754d50f520b3
     Linking ConvertLib to MetaCoin
     Deploying MetaCoin...
     ... 0x5c13ca22826fe4ca6a36c28bd2850bc0f54b38fccd99f43bd928a4cd146b09b0
     MetaCoin: 0x1449e6c720d7501f2b9a54fec8f0123340ac7979
   Saving successful migration to network...
     ... 0x7cfcf52e4ad1548ce92d7a640690366121b9262fca6c141cef6e24f2fd610478
   Saving artifacts...
   
   ```
