# Doichain Environment via Docker Compose 

This repository provides the necessary Docker Compose file, Dockerfiles and/or images to start a complete Doichain Node environment including:
- Doichain Core Node
- P2Pool (P2P Merge Mining Pool to merge mine Bitcoin and Doichain)
- Bitcoin Core Node (pruned) dependency to merge mine Doichain via P2Pool
- Doichain dApp (for Email Marketing)
- MongoDB (dependency for Doichain dApp)
- (planned) ElectrumX Server
- (planned) Mail Server (dependency for Doichain dApp)

## Prerequisite 
1. [Docker](https://docs.docker.com/engine/install/): version 16 or higher 
2. [Docker-Compose](https://docs.docker.com/compose/install/): verison ~1.27 or higher 

## Usage for p2pool mining DOI and BTC 
1. Clone this repo or download this file 
    - ```wget https://raw.githubusercontent.com/inspiraluna/doichain-install/main/docker-compose.yml```
2. Edit docker-comopose.yml 
    1. disable service dapp (comment out the whole block)
    2. change environment variables of p2pool service 
        - P2POOL_DOICHAIN_DEFAULT_ADDR and 
        - P2POOL_BITCOIN_DEFAULT_ADDR  
    in order to tell P2pool where to mint the mined coins. 
3. Run ```docker-compose up -d``` in order to start the Doichain Node environment
4. Run ```docker-compose down``` in order to stop the Doichain Node environment

***Remark***
When starting ```docker compose up``` the bitcoin service downloads a pruned bitcoin blockchain. This takes a while. It will be extracted into the bitcoin docker container. The p2pool service is then showing errors in the logs that it can't connect to bitcoin rpc! (see: ```docker compose exec p2pool tail -f /home/p2pool/nohup.out```) 
1. You can connect to the bitcoin container with ```docker compose exec bitcoin bash``` and ```cd .bitcoin``` and check if the blockchain was already downloaded completely and all blocks synchronized.
2. If the blockchain was downloaded it will sync the missing blocks. You can watch the process via ```docker compose exec bitcoin tail -f /home/bitcoin/.bitcoin/debug.log```
3. As soon as p2pool, bitcoind and doichaind service is running, p2pool mining pool can be access via the ip of the node and port 9332!
4. Bitcoind rpc running on standard port 8332 (Bitcoin p2p on default 8333)
5. Doichain rpc running on standard port 8338 (Docihain p2p on default 8339)

## Usage for Email Double Opt-In request server (you want a Double Opt-In) for your customers or email partners
1. Clone this repo or download this file 
    - ```wget https://raw.githubusercontent.com/inspiraluna/doichain-install/main/docker-compose.yml```
2. Edit docker

## Usage for Email Double-Opt validator (you validate your own DOIs of your own email domains)
1. Clone this repo or download this file 
    - ```wget https://raw.githubusercontent.com/inspiraluna/doichain-install/main/docker-compose.yml```
2. Edit docker-comopose.yml 
    1. disable services p2pool and bitcoin (comment out boths block)
    2. change environment variables dApp service
        - DAPP_SMTP_HOST=your smtp server (e.g. google mail)
        - DAPP_SMTP_USER=your smtp user (e.g. google username)
        - DAPP_SMTP_PASS=your smtp password 
        - DAPP_SMTP_PORT=your smtp port (e.g. 25,587)
        - DAPP_SMTP_DEFAULT_FROM=the email address which is going to be used when sending Double Opt-In confirmation email to your email ussers
    3. Run ```docker-compose up -d``` in order to start the Doichain Node environment
    4. Run ```docker-compose down``` in order to stop the Doichain Node environment


## General usage examples 
### Basics to navigate with Doichain and Docker compose
- show running containers: ```docker-compose ps```
- show all logs of running containers ```docker-compose logs``` 
- connect to a container ```docker-compose exec <containerId> bash``` (or command)

### Basics to navigate on Doichain Node
1. Connect to Doichain Container via ```docker-compose exec doichain bash````
2. Inside of the container you can use the doichain-cli commands such as:
    - doichain-cli help
    - doichain-cli getblockchaininfo
    - doichain-cli getpeerinfo
    - doichain-cli createwallet
    - doichain-cli getbalance
    - doichain-cli getnewaddress
    - doichain-cli getbalance
    - doichain-cli listtransactions
    - doichain-cli gettransaction
    - doichain-cli getrawtransaction
    - doichain-cli getrawmempool

## Basics to navigate with Doichain P2Pool and Doichain Bitcoind
1. When starting Bitcoind first time, it downloads a pruned Bitcoin blockchain and starts syncing the last couple of blocks - please be patients and have a look on the following logs.
2. Check p2pool log ```docker compose exec p2pool tail -f /home/p2pool/nohup.out```
    - is p2pool connected to bitcoin? Or still showing "Bitcoin Core is in initial sync and waiting for blocks..."
3. Check bitcoind log ```docker compose exec bitcoin tail -f /home/bitcoin/.bitcoin/debug.log``` 

## Basics to navigate with Doichain dApp 
1. Connect to Doichain-dApp Container via ```docker-compose exec dapp bash```
2. Connect to Doichain-dApp via browser http://localhost:3000 



