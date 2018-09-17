# 创建一个节点并挖矿  
#### 创建一个节点
* 把编译好的 geth 配置到环境变量  
```
export GETH=你的 GoPath 
export PATH=$GETH/bin:$PATH
```  

* 执行 `mkdir ~/privatechain`  
* 进入到 `privatechain` 目录,执行 `mkdir data0`,继续执行 `vim genesis.json`, 将以下代码复制到 `genesis.json` ,保存后退出 vim 环境   

```
{
  "config": {
    "chainId": 123456,
    "homesteadBlock": 0,
    "eip155Block": 0,
    "eip158Block": 0
  },
  "nonce": "0x0000000000000042",
  "difficulty": "0x020000",
  "mixhash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "coinbase": "0x0000000000000000000000000000000000000000",
  "timestamp": "0x00",
  "parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
  "extraData": "0x11bbe8db4e347b4e8c937c1c8370e4b5ed33adb3db69cbdb7a38e1e50b1b82fa",
  "gasLimit": "0x4c4b40",
  "alloc": {}
}
```  
* 创建一个节点: 在 `privatechain`下执行 `geth --datadir data0 init genesis.json`,若出现如下信息,则创建成功:

```
INFO [09-17|16:38:59.619] Maximum peer count                       ETH=25 LES=0 total=25
INFO [09-17|16:38:59.627] Allocated cache and file handles         database=/Users/hjm/privatechain/data0/geth/chaindata cache=16 handles=16
INFO [09-17|16:38:59.631] Writing custom genesis block
INFO [09-17|16:38:59.631] Persisted trie from memory database      nodes=0 size=0.00B time=12.014µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [09-17|16:38:59.631] Successfully wrote genesis state         database=chaindata                                    hash=611596…424d04
INFO [09-17|16:38:59.631] Allocated cache and file handles         database=/Users/hjm/privatechain/data0/geth/lightchaindata cache=16 handles=16
INFO [09-17|16:38:59.633] Writing custom genesis block
INFO [09-17|16:38:59.633] Persisted trie from memory database      nodes=0 size=0.00B time=3.231µs  gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [09-17|16:38:59.634] Successfully wrote genesis state         database=lightchaindata

```  

* 启动节点并进入到控制台: `geth --datadir data0 --networkid 110 console`. 若出现如下信息,则表示启动成功并进入到了控制台:  

```
INFO [09-17|16:45:24.464] Maximum peer count                       ETH=25 LES=0 total=25
INFO [09-17|16:45:24.473] Starting peer-to-peer node               instance=Geth/v1.8.16-unstable/darwin-amd64/go1.10.1
INFO [09-17|16:45:24.473] Allocated cache and file handles         database=/Users/hjm/privatechain/data0/geth/chaindata cache=768 handles=1024
INFO [09-17|16:45:24.485] Initialised chain configuration          config="{ChainID: 123456 Homestead: 0 DAO: <nil> DAOSupport: false EIP150: <nil> EIP155: 0 EIP158: 0 Byzantium: <nil> Constantinople: <nil> Engine: unknown}"
INFO [09-17|16:45:24.485] Disk storage enabled for ethash caches   dir=/Users/hjm/privatechain/data0/geth/ethash count=3
INFO [09-17|16:45:24.485] Disk storage enabled for ethash DAGs     dir=/Users/hjm/.ethash                        count=2
INFO [09-17|16:45:24.486] Initialising Ethereum protocol           versions="[63 62]" network=110
INFO [09-17|16:45:24.487] Loaded most recent local header          number=0 hash=611596…424d04 td=131072
INFO [09-17|16:45:24.487] Loaded most recent local full block      number=0 hash=611596…424d04 td=131072
INFO [09-17|16:45:24.487] Loaded most recent local fast block      number=0 hash=611596…424d04 td=131072
INFO [09-17|16:45:24.487] Regenerated local transaction journal    transactions=0 accounts=0
INFO [09-17|16:45:24.487] Starting P2P networking
INFO [09-17|16:45:26.596] UDP listener up                          self=enode://9cef69d79933eee4fc17dbde82588d68513458542a7a0b2074c45600b6211ab9b4f27a2cc34654fc9f66ac5106e32f88b684be1d00eaf55e6d69d3e2a02e8f2f@[::]:30303
INFO [09-17|16:45:26.596] RLPx listener up                         self=enode://9cef69d79933eee4fc17dbde82588d68513458542a7a0b2074c45600b6211ab9b4f27a2cc34654fc9f66ac5106e32f88b684be1d00eaf55e6d69d3e2a02e8f2f@[::]:30303
INFO [09-17|16:45:26.598] IPC endpoint opened                      url=/Users/hjm/privatechain/data0/geth.ipc
Welcome to the Geth JavaScript console!

```

#### 在打开的控制台查看并创建账户,查看余额,挖矿等  
* 查看账户:  


```
eth.accounts
``` 
* 创建账户:  


```
personal.newAccount() (要输入两次密码) 
```
* 查看账户余额:  


```
eth.getBalance(eth.accounts[0])
```
*  挖矿:  

```
miner.start(3) (启动三个线程挖矿) 
``` 
* 停止挖矿:  


```
miner.stop()
```


#### 交易  
* 解锁发送交易的账户:

```
personal.unlockAccount(eth.accounts[0])
```
* 发送交易(会返回交易的hash):  


```
eth.sendTransaction({from: eth.accounts[0], to : eth.accounts[1], value: web3.toWei(10, "ether")})
``` 

* 查看交易池中等待被打包的交易:  

```
txpool.status 
```
* 查看等待被打包的交易的详情:  

```
txpool.inspect.pending
```  

* 再次挖矿,让交易被处理(启动挖矿，然后等待挖到一个区块链后就停止):  

```  
miner.start(1); admin.sleepBlocks(1); miner.stop();
```  
* 根据发送交易返回的 hash 查询该交易详情:  

```
eth.getTransaction('0xbc17f40cf592db2915f2fd4319f6802dce4151d48fb7f397c2ff1594b990bdd5')
```  
返回数据:  


```
{
  blockHash: "0xb25d798efc3286fe80f22833e33512d27aac6e00f4119aa0fb5e577b0dd95fee",
  blockNumber: 118,
  from: "0x5fb69cbb4c484c73d387cb8d4f26f1aa54ebb983",
  gas: 90000,
  gasPrice: 1000000000,
  hash: "0xbc17f40cf592db2915f2fd4319f6802dce4151d48fb7f397c2ff1594b990bdd5",
  input: "0x",
  nonce: 0,
  r: "0xa459a5b672bab67a19cdbe16400d8014edbac81bdc7a314f450b2d88a19c5e3a",
  s: "0x2a175cdb37ea4b46ae66e3a999ce540310fce85f88124576055b7fad966ea834",
  to: "0x37cd5ae941df0c0f5b6dcf330ff42a36cb75d1d4",
  transactionIndex: 0,
  v: "0x3c4a4",
  value: 10000000000000000000
}
```  

* 根据区块 id 查询区块的详情  


```
eth.getBlock(118)
```  
返回数据:  

```
{
  difficulty: 133829,
  extraData: "0xd983010810846765746888676f312e31302e318664617277696e",
  gasLimit: 5610180,
  gasUsed: 21000,
  hash: "0xb25d798efc3286fe80f22833e33512d27aac6e00f4119aa0fb5e577b0dd95fee",
  logsBloom: "0x00000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000",
  miner: "0x5fb69cbb4c484c73d387cb8d4f26f1aa54ebb983",
  mixHash: "0xc7a340f4d68158f6f2c6c2e6370b04e3cd39df533979d0d2aea6741a73ec9fa4",
  nonce: "0x1a53397e21a75518",
  number: 118,
  parentHash: "0x3c685a0f143fb895d970f929d9c12eff79c16053d20b7e788ff63af9c204831c",
  receiptsRoot: "0xc552878071f96f77f3ae1edeeedcb761814b864eb2266cf12bc69b00f67b8549",
  sha3Uncles: "0x1dcc4de8dec75d7aab85b567b6ccd41ad312451b948a7413f0a142fd40d49347",
  size: 653,
  stateRoot: "0x92261c5d5ba70a7b0e096dc4cfd6e050f9bcaee97859f6a9316b594fa981212c",
  timestamp: 1537185304,
  totalDifficulty: 16039808,
  transactions: ["0xbc17f40cf592db2915f2fd4319f6802dce4151d48fb7f397c2ff1594b990bdd5"],
  transactionsRoot: "0x32551d2bbdbfa7ec538d7b3865c5288138845d70f6d1fbaf9e7d7fa442984849",
  uncles: []
}
```




