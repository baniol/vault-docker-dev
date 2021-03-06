# Vault Docker Development

## Run

docker-compose up -d

## Initialize Vault


### CLI

install vault cli
wget https://releases.hashicorp.com/vault/0.11.5/vault_0.11.5_linux_amd64.zip
unzip vault_0.11.5_linux_amd64.zip
sudo mv vault /usr/local/bin

export VAULT_ADDR="http://localhost:8200"


### API

curl http://127.0.0.1:8200/v1/sys/seal-status

curl --request PUT -d '{"secret_shares":3, "secret_threshold":2}' http://127.0.0.1:8200/v1/sys/init

{"keys":["7e18e471aa32fd1c8624f0acf465c3e0f4d2bffdcb2ba9023aed0d847d644d2339","36f473f74a9ea959e36e681bc0b5fd4ef859e249ea972406e1d4a3f7f2a43e3f7c","50ce70c1c21ede1bfc3f784550c386f94a4bdfb621b46f68ea6014128e71db2e3e"],"keys_base64":["fhjkcaoy/RyGJPCs9GXD4PTSv/3LK6kCOu0NhH1kTSM5","NvRz90qeqVnjbmgbwLX9TvhZ4knqlyQG4dSj9/KkPj98","UM5wwcIe3hv8P3hFUMOG+UpL37YhtG9o6mAUEo5x2y4+"],"root_token":"1W1OuYGActCjHVyO1SD472pq"}

curl -X PUT -d '{"key": "7e18e471aa32fd1c8624f0acf465c3e0f4d2bffdcb2ba9023aed0d847d644d2339"}' http://127.0.0.1:8200/v1/sys/unseal
curl -X PUT -d '{"key": "36f473f74a9ea959e36e681bc0b5fd4ef859e249ea972406e1d4a3f7f2a43e3f7c"}' http://127.0.0.1:8200/v1/sys/unseal

## Insert data

curl -X POST -H "X-Vault-Token: 1W1OuYGActCjHVyO1SD472pq" -H "Content-Type: application/json" -d '{ "data": { "foo": "world" } }' http://127.0.0.1:8200/v1/secret/data/hello

## Vault UI

http://localhost:8000

## Creating tokens

vault create token -ttl=1m
export VAULT_TOKEN=<token>
vault token lookup

## Links 

* https://www.baeldung.com/vault

## Token & leases

https://learn.hashicorp.com/vault/secrets-management/sm-lease

periodic tokens

vault write auth/token/roles/zabbix allowed_policies="default" period="24h"
vault token create -role test_1