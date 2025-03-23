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
