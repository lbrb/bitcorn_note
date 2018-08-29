# Android接入以太坊节点

### 在build.gradle中添加web3j依赖

```bash
<dependency>
  <groupId>org.web3j</groupId>
  <artifactId>core</artifactId>
  <version>3.3.1-android</version>
</dependency>
```

### 测试环境

```bash
mWeb3j = Web3jFactory.build(new HttpService("http://172.22.33.240:8545/"));
Web3ClientVersion web3ClientVersion = mWeb3j.web3ClientVersion().send();
String clientVersion = web3ClientVersion.getWeb3ClientVersion();
Log.d(TAG, "onClick: "+clientVersion);
```

### 创建钱包及账号

```java
mWalletFileName = WalletUtils.generateNewWalletFile("lb", getWalletsFile(), false);
SharedPreferences sharedPreferences = getSP();
SharedPreferences.Editor editor = sharedPreferences.edit();
editor.putString(mWalletPathKey, getWalletsFile()+File.separator+mWalletFileName);
editor.apply();
```

### 加载账号信息

```java
SharedPreferences sharedPreferences = getSP();
String walletPath = sharedPreferences.getString(mWalletPathKey, "");
if (TextUtils.isEmpty(walletPath)) {
    return "newAccount first";
}
mCredentials = WalletUtils.loadCredentials("lb", walletPath);
String address = mCredentials.getAddress();
BigInteger publicKey = mCredentials.getEcKeyPair().getPublicKey();
BigInteger privateKey = mCredentials.getEcKeyPair().getPrivateKey();
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("address:"+address + "\n\n");
stringBuilder.append("publicKey:"+publicKey + "\n\n");
stringBuilder.append("privateKey:"+privateKey + "\n\n");
Log.d(TAG, "loadWallet: "+stringBuilder.toString());
```

### 查看账号信息

```java
if (mCredentials == null) {
    return "loadWallet first";
}
String address = mCredentials.getAddress();
BigInteger publicKey = mCredentials.getEcKeyPair().getPublicKey();
BigInteger privateKey = mCredentials.getEcKeyPair().getPrivateKey();
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("address:"+address + "\n\n");
stringBuilder.append("publicKey:"+publicKey + "\n\n");
stringBuilder.append("privateKey:"+privateKey + "\n\n");
return stringBuilder.toString();
```

### 查看账号余额

```java
if (mCredentials == null) {
    return "loadWallet first";
}
EthGetBalance ethGetBalance = mWeb3j.ethGetBalance(mCredentials.getAddress(), DefaultBlockParameterName.LATEST).send();
BigInteger balance = ethGetBalance.getBalance();
//Convert.fromWei(balance, Convert.Unit.ETHER).toPlainString()
StringBuilder stringBuilder = new StringBuilder();
stringBuilder.append("address:"+mCredentials.getAddress());
stringBuilder.append("\n\n");
stringBuilder.append("balance: "+balance);
return stringBuilder.toString();
```

