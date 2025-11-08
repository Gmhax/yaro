# yaro

## Download the new update
```
bash -i <(curl -s https://install.aztec.network)
echo 'export PATH="$HOME/.aztec/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
aztec-up latest
aztec-up 2.1.2
```

## Download and install Foundryup
```
 curl -L https://foundry.paradigm.xyz | bash
 source ~/.bashrc
 foundryup
```
- Export your RPC export ETH_RPC=http://your_rpc_here
- Export your Pk export PRIVATE_KEY_OF_OLD_SEQUENCER=yoursequencerPK
## Create your BLS keys
```
aztec validator-keys new \
  --fee-recipient 0x0000000000000000000000000000000000000000000000000000000000000000
```

- Save your Private keys.
`cat /root/.aztec/keystore/key1.json`
- Output like this:
```
{
  "schemaVersion": 1,
  "validators": [
    {
      "attester": {
        "eth": "0x1234567890123456789012345678901234567890123456789012345678901234",
        "bls": "0x2345678901234567890123456789012345678901234567890123456789012345"
      },
      "feeRecipient": "0x0000000000000000000000000000000000000000000000000000000000000000"
    }
  ]
}
```

## Add your address to the validator set
```
aztec \
  add-l1-validator \
  --l1-rpc-urls $ETH_RPC \
  --network testnet \
  --private-key $PRIVATE_KEY_OF_OLD_SEQUENCER \
  --attester <yourAttesterAddress> \
  --withdrawer <yourSequencerAddress> \
  --bls-secret-key <yourBLSSecretKey> \
  --rollup 0xebd99ff0ff6677205509ae73f93d0ca52ac85d67
```
- Replace the following code

<img width="1886" height="56" alt="image" src="https://github.com/user-attachments/assets/a539227e-abea-449c-9733-03b4035909d9" />

- Now check you sequencer address here: https://dashtec.xyz/queue


- if you successfully done creating adding your address on queue follow this.

## Enable Firewall & Open Ports
```
# Firewall
ufw allow 22
ufw allow ssh
ufw enable

# Sequencer
ufw allow 40400
ufw allow 8080
```

- Create aztec directory:
```
mkdir aztec
```
- Get into aztec directory:
```
cd aztec
```
- Create .env
```
nano .env
```
- Replace the following code in .env
```
ETHEREUM_RPC_URL=RPC_URL
CONSENSUS_BEACON_URL=BEACON_URL
VALIDATOR_PRIVATE_KEYS=0xYourAttesterPK
COINBASE=0xYourAttesterAddress
P2P_IP=P2P_IP
GOVERNANCE_PROPOSER_PAYLOAD_ADDRESS=0xDCd9DdeAbEF70108cE02576df1eB333c4244C666
```

## Create docker-compose.yml:
```
nano docker-compose.yml
```
- Replace the following code in docker-compose.yml
```
services:
  aztec-node:
    container_name: aztec-sequencer
    image: aztecprotocol/aztec:2.1.2
    restart: unless-stopped
    environment:
      ETHEREUM_HOSTS: ${ETHEREUM_RPC_URL}
      L1_CONSENSUS_HOST_URLS: ${CONSENSUS_BEACON_URL}
      DATA_DIRECTORY: /data
      VALIDATOR_PRIVATE_KEYS: ${VALIDATOR_PRIVATE_KEYS}
      COINBASE: ${COINBASE}
      P2P_IP: ${P2P_IP}
      LOG_LEVEL: info
    entrypoint: >
      sh -c 'node --no-warnings /usr/src/yarn-project/aztec/dest/bin/index.js start --network testnet --node --archiver --sequencer --sync-mode full'
    ports:
      - 40400:40400/tcp
      - 40400:40400/udp
      - 8080:8080
      - 8880:8880
    volumes:
      - /root/.aztec/testnet/data/:/data
```
## Run Node Docker:
```
docker compose up -d
```
- Node Logs:
```
docker compose logs -fn 1000
```


# DONE



































 
