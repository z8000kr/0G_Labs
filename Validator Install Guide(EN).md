# 0G Newton Validator Installation Guide

-------

## System Requirements
| Category | Requirements |
| ------------ | ------------ |
| CPU | 8+ cores |
| RAM | 64+ GB |
| Storage | 1 TB NVME SSD |
| Bandwidth | 100 MBps for Download / Upload |

### System Update
```bash
sudo apt update && sudo apt upgrade -y
sudo apt install curl git wget htop tmux build-essential jq make lz4 gcc unzip -y
```

### Go Update
```bash
cd $HOME
VER="1.21.3"
wget "https://golang.org/dl/go$VER.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$VER.linux-amd64.tar.gz"
rm "go$VER.linux-amd64.tar.gz"
[ ! -f ~/.bash_profile ] && touch ~/.bash_profile
echo "export PATH=\$PATH:/usr/local/go/bin:~/go/bin" >> ~/.bash_profile
source $HOME/.bash_profile
[ ! -d ~/go/bin ] && mkdir -p ~/go/bin
```

### Git Download and Installation
```bash
git clone -b v0.1.0 https://github.com/0glabs/0g-chain.git
./0g-chain/networks/testnet/install.sh
source .profile
```

### Node Configuration
```bash
echo 'export MONIKER="desired_server_name"' >> ~/.bash_profile
echo 'export CHAIN_ID="zgtendermint_16600-1"' >> ~/.bash_profile
echo 'export WALLET_NAME="wallet"' >> ~/.bash_profile
echo 'export RPC_PORT="26657"' >> ~/.bash_profile
source $HOME/.bash_profile
```

### Server Configuration
```bash
cd $HOME
0gchaind init $MONIKER --chain-id $CHAIN_ID
0gchaind config chain-id $CHAIN_ID
0gchaind config node tcp://localhost:$RPC_PORT
0gchaind config keyring-backend os
```

### Address Book Update
```bash
rm ~/.0gchain/config/genesis.json
curl -Ls https://github.com/0glabs/0g-chain/releases/download/v0.1.0/genesis.json > $HOME/.0gchain/config/genesis.json
curl -Ls https://raw.githubusercontent.com/Core-Node-Team/Testnet-TR/main/0G-Newton/addrbook.json > $HOME/.0gchain/config/addrbook.json
```

### Peers and Seed Update
```bash
# Peers and seeds are updated in the configuration file.
```

### Latest Snapshot Download
```bash
wget http://snapshots.liveraven.net/snapshots/testnet/zero-gravity/zgtendermint_16600-1_latest.tar.lz4
lz4 -d -c ./zgtendermint_16600-1_latest.tar.lz4 | tar -xf - -C $HOME/.0gchain
```

### Service Creation
```bash
# A service for the OG Node is created.
```

### Service Start
```bash
# The service is started and monitored.
```

### Node Sync Check
```bash
# The node's synchronization status is checked.
```

### Wallet Creation
```bash
0gchaind keys add desired_wallet_name --eth
```

### Private Key Generation (For Adding to MetaMask)
```bash
0gchaind keys unsafe-export-eth-key desired_wallet_name
```

### EVM (0x Address) Check (For Receiving Test Tokens)
```bash
0gchaind debug addr $(0gchaind keys show desired_wallet_name -a) | grep 'Address (hex):' | awk -F ': ' '{print "0x" $2}'
```

### Receive Test Tokens
```bash
https://faucet.0g.ai/
```

### Check Wallet Balance
```bash
0gchaind q bank balances $(0gchaind keys show desired_wallet_name -a)
```

### Validator Creation
```bash
# The command to create a validator is provided.
```

### Check Validator Registration: Search by Server Name
```bash
https://zscan.xyz
```
