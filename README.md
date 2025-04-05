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
```

```
mv junctiond-linux-amd64 /usr/local/bin/junctiond  
junctiond init <your-moniker> --chain-id varanasi-1 --default-denom uamf
```

**Download genesis file**
```
wget -O $HOME/.junctiond/config/genesis.json https://raw.githubusercontent.com/Apollo-Sync/Airchain-varanasi/refs/heads/main/genesis.json
```

**Download Addrbook file**
```
wget -O $HOME/.junctiond/config/addrbook.json https://raw.githubusercontent.com/Apollo-Sync/Airchain-varanasi/refs/heads/main/addrbook.json
```


**add peer to file config**
```
PEERS="f5de13c155a191dddd84f6605e04d1c726539e62@152.53.125.167:26656,97027438ed3960132e22d39f343c2158ae7d749d@167.235.14.83:11956,491b207473ce92a8449af71954668f15ec492f16@37.221.198.137:26656,ad40b693a907181cad7f9db73ae7590206418d5e@65.109.84.33:26756,dd56c40aaf17f2d85debdce58fdd139e66a3d528@65.21.192.60:26656,79f26210777e84efb600bf776c32615a72675d9f@airchains-testnet-rpc.itrocket.net:19656,d4fd89f3e5b96be9c1ebf86fb5f3d5dd4059332f@5.9.87.231:36656,b107bf75ca12c4f5fa544390e27f8104b13c7f1b@airchains-testnet-rpc.stakerhouse.com:13756,97027438ed3960132e22d39f343c2158ae7d749d@rpc.airchains.aknodes.net:11956,3039c0c3ba5f12ffe632e84706b52e960f5da595@65.21.202.124:24656,fa83cc2c8ecc7625454b202368b9c7a366bddb91@116.202.150.231:26656,0b9bc2f3fc252e4c087ed495bdb43a372703fb8c@116.202.210.177:26656,5510914e1271930d8f21352e1d887c5e239f4041@144.76.106.228:26656,c0f3abcd838aeb72f6c7a1c817407bfe021547f3@135.181.139.249:26656,029c4e417a43e902575484af0076f1bcd4f664a6@65.108.199.62:29656,ca449bd16b6cfa4e4d6e06fb5eea9a049d58cdac@94.130.239.53:17656,b5d898c94fa206c0eeb130134299c8c4985faec8@65.21.85.184:26656,60cdaad35b5c203fc2c95af04226f4663128775c@148.251.235.130:24656,bb3560a4e8314317259d9a2c6bd7402111d38a1b@149.50.101.137:12356"
sed -i -e "/^\[p2p\]/,/^\[/{s/^[[:space:]]*persistent_peers *=.*/persistent_peers = \"$PEERS\"/}" $HOME/.junctiond/config/config.toml
```

**Create service**
```
sudo tee /etc/systemd/system/junctiond.service > /dev/null << EOF

[Unit]
Description=Junctiond Daemon
#After=network.target
StartLimitInterval=350
StartLimitBurst=10

[Service]
Type=simple
User=root
ExecStart=/usr/local/bin/junctiond start
Restart=on-abort
RestartSec=30

[Install]
WantedBy=multi-user.target

[Service]
LimitNOFILE=1048576
EOF
```

**Reload + enbale**
```
sudo systemctl daemon-reload
sudo systemctl enable junctiond
```
**Start node**
```
sudo systemctl start junctiond
```

**check log**
```
sudo journalctl -u junctiond -fo cat
```
====================================================================================================================================================================================
# SNAPSHOT

```
sudo apt update
sudo apt install lz4
```

```
sudo systemctl stop junctiond
cp $HOME/.junctiond/data/priv_validator_state.json $HOME/.junctiond/priv_validator_state.json.backup
rm -rf $HOME/.junctiond/data
curl https://server-3.itrocket.net/testnet/airchains/airchains_2025-04-05_212729_snap.tar.lz4 | lz4 -dc - | tar -xf - -C $HOME/.junctiond
mv $HOME/.junctiond/priv_validator_state.json.backup $HOME/.junctiond/data/priv_validator_state.json
sudo systemctl restart junctiond && sudo journalctl -u junctiond -f
```
====================================================================================================================================================================================
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
