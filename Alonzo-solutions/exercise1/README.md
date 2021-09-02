#  Exercise 1
## Alonzo Purple testnet

Building a Cardano passive-node and the **cardano-cli** using Nix

#### 1. Fork [cardano-node](https://github.com/input-output-hk/cardano-node)
Clone the IOHK repo for the cardano-node

    git clone git@github.com:input-output-hk/cardano-node.git

#### 2. Checkout to the right tag

For the Alonzo Purple testnet we will be working with a particular tag, checkout to `tag\alonzo-purple-1.0` and create a new branch `1.29.0-rc3-alonzo-purple` 

    git checkout tag/1.29.0-rc3 -b 1.29.0-rc3-alonzo-purple

#### 3. Build node and cli

We now can use Nix to build the node and the cli

    nix-build -A scripts.alonzo-purple.node -o result/alonzo-purple/cardano-node-alonzo-purple
    nix-build -A cardano-cli -o result/alonzo-purple/cardano-cli

#### 4. Add the cli SOCKET

To make quicker access to the cli, we have to add its socket to our `bashrc`. Replace `yourPath` with the path where you have cloned the repo in step 1

    echo export CARDANO_NODE_SOCKET_PATH=~/yourPath/cardano-node/result/alonzo-purple/state-node-alonzo-purple/node.socket >> ~/.bashrc
    source ~/.bashrc

Additionally you can copy `cardano-cli`(see **NOTE**) and `cardano-node-alonzo-purple` bins to your `/usr/local/bin/` path for quick access to these commands as follows

```
$ cd /usr/local/bin/
$ sudo cp ~/yourPath/cardano-node/result/alonzo-purple/cardano-cli/bin/cardano-cli ./
$ sudo cp ~/yourPath/cardano-node/result/alonzo-purple/cardano-node-alonzo-purple/bin/cardano-node-alonzo-purple ./
```

now verify that the commands are accessible, expected output:

```
$ cardano-cli --version
cardano-cli 1.28.0 - linux-x86_64 - ghc-8.10
git rev c17315f2775eaf988e432b7caea3a094d62ce6c9

$ cardano-node-alonzo-purple --help
-Starting: /nix/store/4s7q2xngxq44h6dxgyinrb6f0cac9ajz-cardano-node-exe-cardano-node-1.28.0/bin/cardano-node run
--config /nix/store/nwhbllpv4mvaxdwf6n25bmxnpv0ag0hz-config-0-0.json
--database-path state-node-alonzo-purple/db-alonzo-purple
--topology /nix/store/6r19xjl4i4iaa4nipl9vqqvacqr8fqya-topology.yaml
--host-addr 0.0.0.0
--port 3001
--socket-path state-node-alonzo-purple/node.socket
.
.
.
```

**NOTE.-** Whenever there is an update to the testnet that implies updating the nodes, you should as well remove and update your socket and cli, repeating Step 4.

#### 5. Run a node

We will start a passive-node, this is a relay that can communicate with the testnet but will not participate in the creation of blocks. 

If you are joining before the Hard Fork (HF), before afternoon UTC time of July 14, 2021 you can log all the info of the node to capture the transition of the HF (see exercise 2), you will see the transition from Mary era to Alonzo Purple era.

```
$ cd result/alonzo-purple
$ cardano-node-alonzo-purple
```

You should see a lot of information being printed on the screen.

#### 6. Query last block (new Terminal)

To verify that we are in sync with the testnet we will use the cli and query the tip of the blockchain. Leave the node running and in a new Terminalenter

    $ cardano-cli query tip --testnet-magic 7

we should see something like this

```
{
    "epoch": 194,
    .
    .
    .
    "era": "Alonzo"
}
```

wait a moment and query the newest tip again

```
{
    "epoch": 264,
    .
    .
    .
    "era": "Alonzo"
}
```

Great, the node is up and running!

#### 7. Generate keys

Lastly we will generate payment & stake keys as well as payment & stake addresses. 

To generate a payment address follow

```
## generate payment verification and signing keys
cardano-cli address key-gen \
--verification-key-file payment.vkey \
--signing-key-file payment.skey

## generate stake verification and signing keys
cardano-cli stake-address key-gen \
--verification-key-file stake.vkey \
--signing-key-file stake.skey

## generate payment address (from payment verification and stake verification keys)
cardano-cli address build \
--payment-verification-key-file payment.vkey \
--stake-verification-key-file stake.vkey \
--out-file payment.addr \
--testnet-magic 7
```

Print the payment address on Terminal, and then export it

```
cat payment.addr
addr_test1qzcejjek8d7ah5pnk5lapvq9gjk8hr5t0kyepe0yltusruw04ecqwsgesl2sl65kjyxqkxj24mdgwdpd8e2n6np04u5smq6c3r
export ADDRESS=addr_test1qzcejjek8d7ah5pnk5lapvq9gjk8hr5t0kyepe0yltusruw04ecqwsgesl2sl65kjyxqkxj24mdgwdpd8e2n6np04u5smq6c3r
```

Now generate a staking address

```
cardano-cli stake-address build \
--stake-verification-key-file stake.vkey \
--out-file stake.addr \
--testnet-magic 7
```

verify the staking address

```
cat stake.addr
stake_test1ur86uuq8gyvc04g0a2tfzrqtrf92ak58xsknu4fafsh672g3kvfmg
```

Sweet!

Noy claim your goody-bag of test-ADA!
