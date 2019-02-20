### 准备工作

#### 安装 cdt

```
brew tap eosio/eosio.cdt
brew install eosio.cdt
```

#### 初始化单机环境

1. 获取镜像

```
docker pull eosio/eos-dev:v1.5.2
docker network create eosdev
```

2. 启动节点和钱包

```
docker run \
  --name nodeos -d -p 8888:8888 \
   --network eosdev \
  -v /tmp/eosio/work:/work \
  -v /tmp/eosio/data:/mnt/dev/data \
  -v /tmp/eosio/config:/mnt/dev/config \
  eosio/eos-dev:v1.5.2 \
/bin/bash -c \
  "nodeos -e -p eosio \
    --plugin eosio::producer_plugin \
    --plugin eosio::history_plugin \
    --plugin eosio::chain_api_plugin \
    --plugin eosio::history_api_plugin \
    --plugin eosio::http_plugin \
    -d /mnt/dev/data \
    --config-dir /mnt/dev/config \
    --http-server-address=0.0.0.0:8888 \
    --access-control-allow-origin=* \
    --contracts-console \
    --http-validate-host=false
    --filter-on='*'"

docker run -d -p 9876:9876 --name keosd --network=eosdev \
-i eosio/eos-dev:v1.5.2 /bin/bash -c "keosd --http-server-address=0.0.0.0:9876"
```

3. 检查

```
检查节点：
docker logs --tail 10 eosio


```

4. 创建cleos 别名

```
alias cleos='docker exec -it nodeos /opt/eosio/bin/cleos --url http://localhost:8888 --wallet-url http://172.21.0.3:9876'
```

#### 创建开发者钱包

1. 创建钱包

```
 ./cleos --wallet-url=http://127.0.0.1:8888 wallet create --to-console
 ```
保存好返回的密码 PW5KU9KwhNVN3guaJZU3pDWMVjTqZhqeBmLoKQeZqf49r2zNvjNN4（仅开发模式下使用，生产模式另见文档）

50.72 PW5J6Cfi8RUt3ktQBWAYqgDEnUMGfXwp4gNZnf32JQRxYgq26vWtp

2. 钱包操作

```
打开钱包
cleos wallet open

列举可用钱包列表
cleos wallet list

解锁钱包
./cleos --wallet-url=http://127.0.0.1:8888 wallet unlock
```

3. 导入密钥


```
./cleos --wallet-url=http://127.0.0.1:8888 wallet import
```

system user: eosio 5KQwrPbwdL6PhXujxW37FSSQZ1JiwsST4cqQzDeyXtP79zkvFD3 生产环境无法使用

EOS 的账户创建需要关联钱包公钥，并且同一个钱包公钥可以创建多个账户，但是各个账户是独立的

alice: 5HzbegC64rpPPf9BdHgMA8KWW5obBV89z9YyaA6z1LbQmZeSKkC

bob: 5HzbegC64rpPPf9BdHgMA8KWW5obBV89z9YyaA6z1LbQmZeSKkC

4. 创建测试账户

```
cleos --wallet-url=http://127.0.0.1:8888  create account eosio alice EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio bob EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio xiaohuo EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio xiyu EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio yezong EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio yewei EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio laoma EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio zyp EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio dong  EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888  create account eosio gujie  EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.token EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.msig EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.ram EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.ramfee  EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.stake EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
./cleos --wallet-url=http://127.0.0.1:8888 create account eosio eosio.system EOS5Cs5JmYnSZ2KSid9mJsXgaLk79pdnQNm7Jz47yQvZc4c1vs9kL
```

单机测试情况下使用账户 eosio 来创建账户，公网环境下需要已经存在的账户去创建

### 打包编译

#### 生成 wasm 和 abi

```
eosio-cpp -I include -o eosio.token.wasm src/eosio.token.cpp --abigen
```

### 部署智能合约(注意这里是docker环境，所以wasm 和abi 需要放在挂载的目录)

```
./cleos --wallet-url=http://127.0.0.1:8888 set contract eosio.token contract/eosio.token --abi eosio.token.abi -p eosio.token@active
./cleos --wallet-url=http://127.0.0.1:8888 set contract eosio  contract/eosio.system --abi eosio.system.abi -p eosio@active
```

### 测试

#### 创建 token

```
./cleos  --wallet-url=http://127.0.0.1:8888 push action eosio.token create '[ "eosio.token", "1000000000.0000 EOS"]' -p eosio.token@active
```

#### 发行 token

```
./cleos  --wallet-url=http://127.0.0.1:8888 push action eosio.token issue '[ "alice", "100000000.0000 EOS", "memo" ]' -p eosio.token@active
```

#### 查询 alice 的token

```
cleos  --wallet-url=http://127.0.0.1:8888 get currency balance eosio.token bob EOS
```

#### 转账 alice -> bob

```
cleos  --wallet-url=http://127.0.0.1:8888 push action eosio.token transfer '[ "alice", "gujie", "250000.0000 EOS", "m" ]' -p alice@active
```

#### 查询alice 和 bob 的token

```
./cleos  --wallet-url=http://127.0.0.1:8888 get currency balance eosio.token alice EOS
./cleos --wallet-url=http://127.0.0.1:8888 get currency balance eosio.token bob EOS
./cleos --wallet-url=http://127.0.0.1:8888 get currency balance eosio.token pirate.game EOS
./cleos --wallet-url=http://127.0.0.1:8888 get currency balance eosio.token xiaohuo EOS
```

cleos --wallet-url=http://127.0.0.1:8888 system delegatebw alice pirate.game "1000 EOS" "1000 EOS" -p alice@active

cleos --wallet-url=http://127.0.0.1:8888 get abi eosio
cleos --wallet-url=http://127.0.0.1:8888 set contract eosio eosio.system



## update!!!

nohup ./nodeos -e -p eosio --plugin eosio::chain_api_plugin --plugin eosio::history_api_plugin --plugin eosio::wallet_plugin --plugin eosio::wallet_api_plugin &
