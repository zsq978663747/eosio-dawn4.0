# eosio-dawn4.0

1.节点启动：启动了三个节点
```
eosio：
 ./nodeos -d ~/eos.data/generator_node --config-dir ~/eos.data/generator_node   --plugin eosio::wallet_api_plugin --plugin eosio::chain_api_plugin --p2p-peer-address localhost:9876 --p2p-listen-endpoint localhost:5555 
```
```
bp1：
 ./nodeos -d ~/eos.data/generator_node --config-dir ~/eos.data/generator_node   --plugin eosio::wallet_api_plugin --plugin eosio::chain_api_plugin --p2p-peer-address localhost:9876   --p2p-peer-address localhost:4444    --p2p-listen-endpoint localhost:5555 
```
```
bp2：
 ./nodeos -d ~/eos.data/bp2_node --config-dir ~/eos.data/bp2_node   --plugin eosio::wallet_api_plugin --plugin eosio::chain_api_plugin --p2p-peer-address localhost:9876   --p2p-peer-address localhost:5555    --p2p-listen-endpoint localhost:4444 
```
```
./nodeos -d /root/EOS-Boot-Steps-dawn4/  --config-dir  /root/EOS-Boot-Steps-dawn4/config
```
其中还要配置config文件，把BP1 和BP2 的秘钥加入到对应的config文件中，这里的秘钥也是后面创建bp，并且注册生产者的秘钥。
private-key = ["EOS5JaXkrizggfLXoQ5qACvZJVCheJdR3ipKJk6pnF7o5N5uaseMP","5KZeRRaMw5QpWvwSpRxCK1ib3UyqZaQ6mDLdmJRZMvuQk72P6KJ"]


2.创建钱包
```
./cleos wallet create PW5KHHECv4bJxXsLocmsSnkdMvMBood5Tnd3KYmWEySfnvY1bqe9b
./cleos wallet unlock --password  PW5KHHECv4bJxXsLocmsSnkdMvMBood5Tnd3KYmWEySfnvY1bqe9b
```
3.部署eosio合约

系统部署合约之前都需要部署一个bios合约
```
$./cleos set contract eosio ../../contracts/eosio.bios -p eosio
```
4.导入私钥，创建eosio.token账户,部署合约
```
$./cleos wallet import 5JkgLHjSHmT8pWD5vKfH6vjW5jRtekx37rqZpbwF2Au251xqT41
$./cleos create account eosio eosio.token EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42 EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42
```
部署eosio.token合约
```
$./cleos set contract eosio.token ../../contracts/eosio.token/ -p eosio.token
```


5.导入私钥，创建两个bp
```
./cleos wallet import 5Kc4kYKiDMHdamai8QHcZ7jTUN5JvbLQJMZTSGtsEPXrZRqypnM
EOS6FhDpkkBmZPBrANjfU7z8rBDgm8xwaYLAg8qBsLYR2AG25APcb 

./cleos wallet import  5KZeRRaMw5QpWvwSpRxCK1ib3UyqZaQ6mDLdmJRZMvuQk72P6KJ
EOS5JaXkrizggfLXoQ5qACvZJVCheJdR3ipKJk6pnF7o5N5uaseMP 

./cleos create account eosio bp1 EOS6FhDpkkBmZPBrANjfU7z8rBDgm8xwaYLAg8qBsLYR2AG25APcb EOS6FhDpkkBmZPBrANjfU7z8rBDgm8xwaYLAg8qBsLYR2AG25APcb

./cleos create account eosio bp2 EOS5JaXkrizggfLXoQ5qACvZJVCheJdR3ipKJk6pnF7o5N5uaseMP EOS5JaXkrizggfLXoQ5qACvZJVCheJdR3ipKJk6pnF7o5N5uaseMP 
```
6.给bp1，bp2发布eos代币
```
./cleos push action eosio.token create '{"issuer":"eosio", "maximum_supply": "1000000000.0000 EOS", "can_freeze": 0, "can_recall": 0, "can_whitelist": 0}' -p eosio.token
./cleos push action eosio.token issue '{"to":"bp1","quantity":"100000000.0000 EOS","memo":"issue"}' -p eosio
./cleos push action eosio.token issue '{"to":"bp2","quantity":"100000000.0000 EOS","memo":"issue"}' -p eosio
```
7.部署eosio.system合约
```
./cleos set contract eosio ../../contracts/eosio.system -p eosio
```
8.创建投票账户
```
./cleos  system newaccount eosio voterabcdef1 EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  --stake-net '50.00 EOS' --stake-cpu '50.00 EOS'
./cleos  system newaccount eosio voterabcdef2 EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  --stake-net '50.00 EOS' --stake-cpu '50.00 EOS'
./cleos  system newaccount eosio voterabcdef3 EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  EOS8cMSi7eiGXC8oWcNfYFf83BRToBoKS4Dp46ERA6Hkb1cCwMn42  --stake-net '50.00 EOS' --stake-cpu '50.00 EOS'
```
9.给账户发钱
```
./cleos push action eosio.token issue '{"to":"voterabcdef1","quantity":"100000000.0000 EOS","memo":"issue"}' -p eosio
./cleos push action eosio.token issue '{"to":"voterabcdef2","quantity":"100000000.0000 EOS","memo":"issue"}' -p eosio
./cleos push action eosio.token issue '{"to":"voterabcdef3","quantity":"100000000.0000 EOS","memo":"issue"}' -p eosio
```
10.锁定代币
```
./cleos system delegatebw voterabcdef1  voterabcdef1 '25000000.0000 EOS' '25000000.0000 EOS' --transfer
./cleos system delegatebw voterabcdef2 voterabcdef2 '50000000.0000 EOS' '50000000.0000 EOS' --transfer
./cleos system delegatebw voterabcdef3 voterabcdef3 '50000000.0000 EOS' '50000000.0000 EOS' --transfer
```
11.注册bp
```
./cleos system regproducer bp1 EOS6FhDpkkBmZPBrANjfU7z8rBDgm8xwaYLAg8qBsLYR2AG25APcb
./cleos system regproducer bp2 EOS5JaXkrizggfLXoQ5qACvZJVCheJdR3ipKJk6pnF7o5N5uaseMP
```
12.投票
```
./cleos system voteproducer prods voterabcdef1 bp1
./cleos system voteproducer prods voterabcdef2 bp1
./cleos system voteproducer prods voterabcdef3  bp1

./cleos system voteproducer prods voterabcdef1 bp2
./cleos system voteproducer prods voterabcdef2  bp2
./cleos system voteproducer prods voterabcdef3 bp2
```




购买ram
```
./cleos system buyram voter voter '1000 EOS'
```
查看ram
```
./cleos get table eosio  voter userres 
```
出售ram
```
./cleos system sellram voter 9000
```
