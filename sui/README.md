# Sui node setup for devnet

Official doc:
- Official manual: https://docs.sui.io/build/fullnode
- Experiment with Sui DevNet: https://docs.sui.io/explore/devnet
- Check you node health: https://node.sui.zvalid.com/



## Recommended hardware 
- CPU: 2 CPU
- Memory: 8 GB RAM
- Disk: 50 GB SSD Storage

## Set up your Sui full node
### Option 1 (automatic)
You can setup your Sui full node in minutes by using automated script below
```
wget -O sui.sh https://raw.githubusercontent.com/fathoni6/gulonodes/main/sui/sui.sh && chmod +x sui.sh && ./sui.sh
```

## Make tests
Once the fullnode is up and running, test some of the JSON-RPC interfaces.

### Check status of your node
```
curl -s -X POST http://127.0.0.1:9000 -H 'Content-Type: application/json' -d '{ "jsonrpc":"2.0", "method":"rpc.discover","id":1}' | jq .result.info
```

You should see something similar in the output:
```json
{
  "title": "Sui JSON-RPC",
  "description": "Sui JSON-RPC API for interaction with the Sui network gateway.",
  "contact": {
    "name": "Mysten Labs",
    "url": "https://mystenlabs.com",
    "email": "build@mystenlabs.com"
  },
  "license": {
    "name": "Apache-2.0",
    "url": "https://raw.githubusercontent.com/MystenLabs/sui/main/LICENSE"
  },
  "version": "0.1.0"
}
```

### Get the five most recent transactions
```
curl --location --request POST 'http://127.0.0.1:9000/' --header 'Content-Type: application/json' \
--data-raw '{ "jsonrpc":"2.0", "id":1, "method":"sui_getRecentTransactions", "params":[5] }' | jq .
```

### Get details about a specific transaction
```
curl --location --request POST 'http://127.0.0.1:9000/' --header 'Content-Type: application/json' \
--data-raw '{ "jsonrpc":"2.0", "id":1, "method":"sui_getTransaction", "params":["<RECENT_TXN_FROM_ABOVE>"] }' | jq .
```

## Post installation
After setting up your Sui node you have to register it in the [Sui Discord](https://discord.gg/b5vWu33f):
1) navigate to `#📋node-ip-application` channel
2) post your node endpoint url
```
http://<YOUR_NODE_IP>:9000/
```

## Check your node health status
Enter your node IP into https://node.sui.zvalid.com/

## Update Sui Fullnode version
```
wget -qO update.sh https://raw.githubusercontent.com/fathoni6/gulonodes/main/sui/update/update.sh && chmod +x update.sh && ./update.sh
```

## Usefull commands
Check sui node status
```
docker ps -a
```

Check node logs
```
docker logs -f sui-fullnode-1 --tail 50
```

To delete node
```
cd $HOME/sui && docker-compose down --volumes
cd $HOME && rm -rf sui
```