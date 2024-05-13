<h1 align="center"> 0G Newton 밸리데이터 설치 가이드입니다.


</h1>




## 시스템 요구사항
| 구분 | 요구사항 | 
| ------------ | ------------ |
| CPU |	8+|
| RAM	| 64+ GB |
| Storage	| 1 TB NVME SSD |
| Bandwidth	| 100 MBps for Download / Upload |

### 시스템 업데이트
```
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

### go 업데이트
```
cd $HOME
VER="1.21.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

### git 다운로드 설치
```
git clone -b v0.1.0 https://github.com/0glabs/0g-chain.git
./0g-chain/networks/testnet/install.sh
source .profile
```

### 노드설정
```
echo 'export MONIKER="원하는서버이름"' >> ~/.bash_profile
echo 'export CHAIN_ID="zgtendermint_16600-1"' >> ~/.bash_profile
echo 'export WALLET_NAME="wallet"' >> ~/.bash_profile
echo 'export RPC_PORT="26657"' >> ~/.bash_profile
source $HOME/.bash_profile
```

### 서버설정
```
cd $HOME
0gchaind init $MONIKER --chain-id $CHAIN_ID
0gchaind config chain-id $CHAIN_ID
0gchaind config node tcp://localhost:$RPC_PORT
0gchaind config keyring-backend os
```

### 주소록 업데이트
```
rm ~/.0gchain/config/genesis.json
curl -Ls https://github.com/0glabs/0g-chain/releases/download/v0.1.0/genesis.json > $HOME/.0gchain/config/genesis.json
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/0G-Newton/addrbook.json > $HOME/.0gchain/config/addrbook.json
```

### peers,seed 업데이트
```
PEERS="fbc3b6d41cd39a62ef5e3fc596435adfaf428a34@37.120.189.81:16656,645531eb02b275a59cc3b1af99e541852849f695@84.247.139.25:16656,d00273ac6a2470cd4e48008d9af4d2521b134394@62.169.29.136:26656,f5a7d34355f6d89b7ece583131c6b1f79ac5485e@218.102.97.67:25856,a3e6c6214805c1c068882f1981855c7a9f5926ea@213.168.249.202:26656,da1f4985ce3df05fd085460485adefa93592a54c@172.232.33.25:26656,91f079ccd2e0edf42e0fa57183ac92c22c525658@14.245.25.144:14256,9d09d391b2cf706a597d03fe8bb6700fe5cac53d@65.108.198.183:18456,5a202fb905f20f96d8ff0726f0c0756d17cf23d8@43.248.98.100:26656,74775d65b6ab427c685efcaa8190912d3a60e562@123.19.45.21:12656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,9d7564df34efa146a94c073e5bf3f5e11f947b75@155.133.22.230:26656,e179d05dc792d9b902be3baa7a31a07a92afbcf0@118.142.83.5:26656,c4b9c3a7f3651af729d73b150e714ee91e7585c1@14.176.200.133:26656,f64f0fb500c62bffa33d60450d30792ee4b5fbd0@167.86.119.168:26656,d4085fd93ab77576f2acdb25d2d817061db5afe6@62.169.19.156:26656,2b8ee12f4f94ebc337af94dbec07de6f029a24e6@94.16.31.161:26656,0f5022e4265184052a5468379687625a81fd255e@154.12.253.116:26656,3859828e1099214de14dae91d1f7decf2374eeb4@47.236.170.254:26656,23b0a0624699f85062ddebf910583f70a5b9e86b@14.167.152.116:14256,b8f8ed478f2794629fdb5cf0c01edaed80f00f84@168.119.64.172:26656,5d81d59e81356a33e6ccccaa3d419ff73244697e@107.173.18.103:26656,c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,a83f5d07a8a64827851c9f1d0c21c900b9309608@188.166.181.110:26656,19943cbe46cdb9eb37cb06c0067ce63154eee6ea@213.199.52.155:26656,a6ff8a651dd0a0e66dbfb2174ccadcbbcf567b29@66.94.122.224:26656,f3c912cf5653e51ee94aaad0589a3d176d31a19d@157.90.0.102:31656,141dbd90d5c3411c9ba72ba03704ccdb70875b01@65.109.147.58:36656,cd529839591e13f5ed69e9a029c5d7d96de170fe@46.4.55.46:34656,a8d7c5a051c4649ba7e267c94e48a7c64a00f0eb@65.108.127.146:26656" && \
SEEDS="c4d619f6088cb0b24b4ab43a0510bf9251ab5d7f@54.241.167.190:26656,44d11d4ba92a01b520923f51632d2450984d5886@54.176.175.48:26656,f2693dd86766b5bf8fd6ab87e2e970d564d20aff@54.193.250.204:26656,f878d40c538c8c23653a5b70f615f8dccec6fb9f@54.215.187.94:26656" && \
sed -i -e "s/^seeds *=.*/seeds = \"$SEEDS\"/; s/^persistent_peers *=.*/persistent_peers = \"$PEERS\"/" $HOME/.0gchain/config/config.toml
```

### 최신 스냅샷 다운로드
```
wget http://snapshots.liveraven.net/snapshots/testnet/zero-gravity/zgtendermint_16600-1_latest.tar.lz4
```
```
lz4 -d -c ./zgtendermint_16600-1_latest.tar.lz4 | tar -xf - -C $HOME/.0gchain
```

### 서비스 생성
```
sudo tee /etc/systemd/system/0gchaind.service > /dev/null <<EOF
[Unit]
Description=OG Node
After=network.target

[Service]
User=$USER
Type=simple
ExecStart=$(which 0gchaind) start --home $HOME/.0gchain
Restart=10
LimitNOFILE=65535

[Install]
WantedBy=multi-user.target
EOF
```

### 서비스 시작
```
sudo systemctl daemon-reload && \
sudo systemctl enable 0gchaind && \
sudo systemctl restart 0gchaind && sudo journalctl -u 0gchaind -f -o cat
```

### 노드 싱크확인
ctrl+c 로 종료후에 아래 입력하고 false가 될때까지 확인
```
0gchaind status | jq .sync_info
```

### 지갑 생성
```
0gchaind keys add 원하는지갑이름 --eth
```

### 프라이빗키 생성(메타마스크 지갑추가용)
```
0gchaind keys unsafe-export-eth-key 원하는지갑이름
```

### EVM(0x주소) 확인 (테스트토큰 받는 용도)
```
0gchaind debug addr $(0gchaind keys show 원하는지갑이름 -a) | grep 'Address (hex):' | awk -F ': ' '{print "0x" $2}'
```

### 테스트토큰 받기
```
https://faucet.0g.ai/
```

### 지갑 보유량 확인
```
0gchaind q bank balances $(0gchaind keys show 원하는지갑이름 -a)
```

### 벨리데이터 생성
```
0gchaind tx staking create-validator \
  --amount=1000000ua0gi \
  --pubkey=$(0gchaind tendermint show-validator) \
  --moniker=원하는서버이름 \
  --chain-id=zgtendermint_16600-1 \
  --commission-rate=0.05 \
  --commission-max-rate=0.10 \
  --commission-max-change-rate=0.01 \
  --min-self-delegation=1 \
  --from=원하는지갑이름 \
  --identity="" \
  --website="" \
  --details="" \
  -y
```

### 밸리데이터 등록여부 확인 : 서버이름 검색
```
https://dashboard.nodebrand.xyz/0g-chain/staking
```
