# 使用 Safe SDK 创建多签钱包

## Safe 简介

Safe（前身为 Gnosis Safe）是一个建立在以太坊网络上的智能合约钱包平台，专注于提供安全、灵活的数字资产管理解决方案。作为目前以太坊生态中最受信赖的多签钱包解决方案之一，Safe 广泛应用于个人资产管理、DAO 治理、企业财务等领域。

👉 [了解更多区块链安全解决方案](https://bit.ly/okx_welcome)

### 为什么选择 Safe？
- 支持多重签名验证机制
- 提供模块化扩展功能
- 与主流钱包（MetaMask、WalletConnect 等）兼容
- 开放的 SDK 开发接口

---

## 开发准备

### 开发环境要求
- Node.js 14.x 或更高版本
- npm/yarn 包管理工具
- 基础区块链开发知识

### 核心依赖包
| 包名                     | 功能说明                  |
|--------------------------|---------------------------|
| ethers                   | 以太坊交互库              |
| @safe-global/protocol-kit | Safe 协议核心 SDK         |
| @safe-global/api-kit     | Safe API 接口工具         |

---

## 开发实战指南

### 部署多签钱包

```javascript
const { SafeFactory } = require('@safe-global/protocol-kit');
const safeFactory = await SafeFactory.init({
  provider: '<provider url>', 
  signer: '<sender private key>'
})

const safeAccountConfig = {
  owners: [
    '<signer address 1>', 
    '<signer address 2>', 
    '<signer address 3>'
  ],
  threshold: 2
}

const safeWallet = await safeFactory.deploySafe({ safeAccountConfig });
const safeAddress = await safeWallet.getAddress();
console.log('Safe 钱包已部署');
console.log(`https://app.safe.global/sep:${safeAddress}`);
```

#### 部署注意事项
- 确保提供有效的 RPC 节点地址
- 部署账户需有足够的 ETH 支付 gas 费用
- 推荐使用 Sepolia 测试网进行开发测试

👉 [获取以太坊开发资源](https://bit.ly/okx_welcome)

---

### 钱包交互操作

#### 读取钱包信息
```javascript
const Safe = require('@safe-global/protocol-kit').default;
const safeWallet = await Safe.init({
  provider: '<provider url>', 
  safeAddress: '<safeAddress>'
})

const threshold = await safeWallet.getThreshold();
const owners = await safeWallet.getOwners();
console.log(`Threshold: ${threshold}`);
console.log(`Owners: ${owners.join(', ')}`);
```

#### 发起 ETH 转账
```javascript
const signerPrivateKey = "0x..."; 
const safeTransactionData = {
  to: '<receiver>', 
  data: '0x',
  value: ethers.parseUnits('0.01', 'ether').toString()
}

const safeTransaction = await safeWallet.createTransaction({ 'transactions': [safeTransactionData] });
const safeTxHash = await safeWallet.getTransactionHash(safeTransaction);
const senderSignature = await safeWallet.signHash(safeTxHash);
```

---

## 多签交易流程

### 交易确认流程
1. 创建交易提案
2. 获得所需签名数
3. 执行交易上链

```javascript
if (transaction.confirmations.length !== transaction.confirmationsRequired) {
  console.log('未达到签名确认数');
  return;
}
const safeTransaction = await apiKit.getTransaction(transaction.safeTxHash);
const executeTxResponse = await safeWallet.executeTransaction(safeTransaction);
const receipt = await executeTxResponse.transactionResponse?.wait();
console.log('交易已执行，hash：', receipt.hash);
```

---

## 常见问题解答

### Q1：什么是多签钱包的阈值设置？
阈值（threshold）是指执行交易所需获得的最小签名数量。例如 2-3 多签方案意味着 3 个授权地址中至少需要 2 个签名即可执行交易。

### Q2：如何管理多个签名者？
可通过 Safe 管理界面或 SDK 接口进行：
- 添加/移除签名者
- 修改阈值设置
- 查询签名状态

### Q3：交易确认需要多长时间？
通常在达到签名阈值后，交易可在 1-3 个区块时间内完成上链（约 15-45 秒）。

### Q4：如何保障私钥安全？
建议采用：
- 硬件钱包存储
- 环境变量保存私钥
- 使用加密密钥库

👉 [获取区块链安全实践指南](https://bit.ly/okx_welcome)

---

## 进阶应用场景

### 企业级资金管理
- 支持多部门协同审批
- 实现自动化的支出控制
- 与会计系统集成

### DAO 治理方案
- 链上投票结果执行
- 预算分配管理
- 多签提案机制

### 开发者扩展能力
- 自定义模块开发
- 事件监听系统
- 交易批处理功能

---

## 学习资源推荐

官方文档：
- [Safe 开发者指南](https://docs.safe.global/)
- [协议工具包文档](https://docs.safe.global/sdk/overview)

学习建议：
1. 从基础 API 开始实践
2. 逐步尝试模块化开发
3. 参与社区技术讨论

通过 Safe SDK 的强大功能，开发者可以构建更安全可靠的区块链应用。掌握多签钱包的开发技术，将为您打开 Web3.0 世界的更多可能性。