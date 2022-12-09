<p style="font-size:14px" align="right">
<a href="https://github.com/elangrr/testnet_guide" target="_blank">More Guide Tutorials<img src="https://avatars.githubusercontent.com/u/34649601?v=4" width="30"/></a>
</p>

<p style="font-size:14px" align="right">
<a href="https://indonode.dev/" target="_blank">Visit my website <img src="https://avatars.githubusercontent.com/u/34649601?v=4" width="30"/></a>
</p>

<p align="center">
 <img height="200" height="auto" src="https://user-images.githubusercontent.com/34649601/206725652-b4621198-ab00-4943-8cba-b0f47c5a2502.png">


# Official Links
### [Official Document](https://docs.qtestnet.org/how-to-setup-validator/)
### [Q Official Discord](https://discord.gg/nYaCmDw4ku)

# Explorer
### [EXPLORER](https://stats.qtestnet.org/)

## Minimum Requirements 
NOT SPECIFIED 

NOTE : Run as `root` user

### Set vars
```
PASSWORD=YOUR_PASSWORD
```
Change `YOUR_PASSWORD` to secure password

Export the variable 
```
echo "export PASSWORD=$PASSWORD" $HOME/.bash_profile
source $HOME/.bash_profile
```

### Update
```
sudo apt update && \
sudo apt upgrade
```

### Install Docker
```
sudo apt-get update && sudo apt install jq && sudo apt install apt-transport-https ca-certificates curl software-properties-common -y && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - && sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu focal stable" && sudo apt-get install docker-ce docker-ce-cli containerd.io docker-compose-plugin && sudo apt-get install docker-compose-plugin
```
`y` 

### Download Binary and set Password file
```
git clone https://gitlab.com/q-dev/testnet-public-tools.git
cd testnet-public-tools/testnet-validator/
```

```
mkdir keystore
cd keystore/
echo "$PASSWORD" >> pwd.txt
```

### Generate Wallet
```
cd ..
docker run --entrypoint="" --rm -v $PWD:/data -it qblockchain/q-client:testnet geth account new --datadir=/data --password=/data/keystore/pwd.txt
```
SAVE THE OUTPUT!!!

### Fund your Wallet
Fund your wallet [ Q Faucet ](https://faucet.qtestnet.org/)

### Configure
```
cp .env.example .env
nano .env
```

Add Address without `0x` and your Public IP and save settings `CTRL+X` `y` and `ENTER`

Example : 

 <img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206742445-4c80d903-09bd-4e80-ac1f-f8711ec4f8f2.png">

Configure `config.json`
```
nano config.json
```
Add Address without `0x` and your Password from the beginning and save `CTRL+X` `y` and `ENTER`

<img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206742620-1c42d1a7-34c6-4b8c-a1da-95b2d0e3b577.png">

### Stake to the contract
```
docker run --rm -v $PWD:/data -v $PWD/config.json:/build/config.json qblockchain/js-interface:testnet validators.js
```

### Export your private key to Metamask 
```
cd testnet-public-tools
chmod +x run-js-tools-in-docker.sh
./run-js-tools-in-docker.sh
npm install
```
```
chmod +x extract-geth-private-key.js
node extract-geth-private-key <WALLET_ADDRESS> ../testnet-validator/ $PASSWORD
```
Change `<WALLET_ADDRESS>` To your Wallet address

After saving your private key exit the container `exit`

# Registering Validator

In order to register your node you have to register in the Form : [Register Form](https://itn.qdev.li/)

Register your validator according to your validator info

After successfully register your validator you will receive your validator name 

<img height="400" height="auto" src="https://user-images.githubusercontent.com/34649601/206744494-8418897e-226c-49be-a073-4ed939084384.png">

### Configure docker-compose.yaml
```
cd testnet-validator
nano docker-compose.yaml
```
Then add `--ethstats=ITN-testname-ecd07:qstats-testnet@stats.qtestnet.org` 

Example : 

<img height="450" height="auto" src="https://user-images.githubusercontent.com/34649601/206744942-380d5e57-3f93-48f0-a17c-40c623f962b4.png">


Then Run the Validator : 
```
docker compose up -d
```

Check logs 
```
docker compose logs -f
```
CTRL+C to exit logs

Check your validator 

[Q Explorer](https://stats.qtestnet.org/)