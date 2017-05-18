# Ethereum IoT: Creating a Private Blockchain on a Raspberry Pi

Background: A blockchain is a digital ledger where immutable transactions take place. Blockchains are already used in finance as a way to keep track of transactions. Cryptocurrencies like bitcoin is built on top of a blockchain.

Since records committed records can't be tampered with or destroyed, many other use cases of the blockchain are being explored. In the Internet of Things Space, blockchains can be used to 

* [Create an efficient energy grid](https://www.technologyreview.com/s/604227/blockchain-is-helping-to-build-a-new-kind-of-energy-grid/)


* [Monitor and Validate Supply Chains](https://www.mendix.com/blog/built-iot-application-10-days-using-watson-iot-ibm-blockchain/)


* [Securing IoT Devices and decreasing DDoS attacks](https://www.technologyreview.com/s/603298/a-secure-model-of-iot-with-blockchain/)


## What is Parity?

[Parity](https://parity.io/) is the rust client for Ethereum. Parity comes with a rich toolset to help developers interact and build upon the Ethereum blockchain.


## Install Parity on Raspberry Pi

In order to interact with the Ethereum blockchain you will have to compile Parity onto your Raspberry pi. 

```
$ curl https://sh.rustup.rs -sSf | sh
$ source $HOME.cargo/env
$ cargo install --git https://github.com/ethcore/parity.git parity
```

## Quickly Configure Blockchain

Ethereum blockchains are composed of a consensus engine, a name, and a genesis block. The genesis block, as the name suggests, is the first block in your blockchain. All of these components are stored in a genesis file. Below is a bare bones genesis file to bootstrap your private chain. If you want to learn more about consensus engines and other chain parameters, you can head over to [Parity's docs](https://github.com/paritytech/parity/wiki/Chain-specification)

```
{
	"name": "Morden",
	"engine": {
		"Ethash": {
			"params": {
				"gasLimitBoundDivisor": "0x0400",
				"minimumDifficulty": "0x020000",
				"difficultyBoundDivisor": "0x0800",
				"durationLimit": "0x0a",
				"blockReward": "0x4563918244F40000",
				"registrar": "",
				"homesteadTransition": "0x0"
			}
		}
	},
	"params": {
		"accountStartNonce": "0x0100000",
		"maximumExtraDataSize": "0x20",
		"minGasLimit": "0x1388",
		"networkID" : "0x42"
	},
	"genesis": {
		"seal": {
			"ethereum": {
				"nonce": "0x00006d6f7264656e",
				"mixHash": "0x00000000000000000000000000000000000000647572616c65787365646c6578"
			}
		},
		"difficulty": "0x20000",
		"author": "0x0000000000000000000000000000000000000000",
		"timestamp": "0x00",
		"parentHash": "0x0000000000000000000000000000000000000000000000000000000000000000",
		"extraData": "0x",
		"gasLimit": "0x2fefd8"
	},
	"accounts": {
		"0000000000000000000000000000000000000001": { "balance": "1", "nonce": "1048576", "builtin": { "name": "ecrecover", "pricing": { "linear": { "base": 3000, "word": 0 } } } },
		"0000000000000000000000000000000000000002": { "balance": "1", "nonce": "1048576", "builtin": { "name": "sha256", "pricing": { "linear": { "base": 60, "word": 12 } } } },
		"0000000000000000000000000000000000000003": { "balance": "1", "nonce": "1048576", "builtin": { "name": "ripemd160", "pricing": { "linear": { "base": 600, "word": 120 } } } },
		"0000000000000000000000000000000000000004": { "balance": "1", "nonce": "1048576", "builtin": { "name": "identity", "pricing": { "linear": { "base": 15, "word": 3 } } } }
	}
}
```

## Activate Private Blockchain 

In order to get your private blockchain up and running, type parity and pass in the myGenesis.json file as the parameter.

`parity --chain myGenesis.json`
