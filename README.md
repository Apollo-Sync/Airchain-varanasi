# Install node
**Build-Essential and jq Installation**
```
sudo apt update && sudo apt install -y build-essential jq
```
**Go Installation (v1.22+)**
```
wget https://go.dev/dl/go1.22.2.linux-amd64.tar.gz  
sudo tar -C /usr/local -xzf go1.22.2.linux-amd64.tar.gz  
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.bashrc  
source ~/.bashrc  
go version  # Verify installation  
```

**Rust Installation (v1.80+)**
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh 
rustc --version  # Verify installation  
```
Choose the default installation (option 1) when prompted

**Install & Initialize the Node**
```
wget https://github.com/airchains-network/junction/releases/download/v0.3.1/junctiond-linux-amd64  
chmod +x junctiond-linux-amd64  
mv junctiond-linux-amd64 /usr/local/bin/junctiond  
junctiond init <your-moniker> --chain-id varanasi-1 --default-denom uamf
```
**Download genesis file**
```
wget -O $HOME/.junctiond/config/genesis.json https://server-3.itrocket.net/testnet/airchains/genesis.json
```

# Unjail node 

wallet : change your wallet name

keyring-backend file: change your backend file

**Create an unsigned transaction**
```
junctiond tx slashing unjail --from wallet --chain-id varanasi-1 --fees 5000uamf --keyring-dir /root/.junction --keyring-backend file --generate-only > unsigned.txt
```

**Sign the transaction**
```
junctiond tx sign unsigned.txt --from wallet --chain-id varanasi-1 --keyring-dir /root/.junction --keyring-backend file > signed.txt
```

**Broadcast the transaction**
```
junctiond tx broadcast signed.txt
```

**VALIDATOR NODE CHECK**
```
junctiond tx broadcast signed.txt
```
