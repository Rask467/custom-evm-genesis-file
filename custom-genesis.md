# Introduction

In this tutorial you will learn how to create an avalanche subnet with a custom EVM genesis file. This will allow you to customize how your subnet is generated. A custom genesis file gives you full control over many aspects of your subnet including changing fee configuration, adding a deployer allow list, modifying the block difficulty, and more. Let's get started!

# Installing Avalanche-CLI

The [Avalanche-CLI](https://github.com/ava-labs/avalanche-cli) is a great way to get up and running with a subnet.

To download the latest release, run:

```
curl -sSfL https://raw.githubusercontent.com/ava-labs/avalanche-cli/main/scripts/install.sh | sh -s
```

Then add the binary to your path

```
cd bin
export PATH=$PWD:$PATH
```

You've successfully installed the Avalanche-CLI now let's work on creating subnet EVM genesis file. 

# Creating EVM Genesis File

First create a file somehwere called `my-first-genesis.json`

Inside of this file create an empty JSON object

```
{}
```

We will now go through each of the available parameters so you will have the knowledge to customize your genesis file with confidence.

```
{
 "config": {
   ...
   ...
   ...
 }
}
```

## The Config Object

The first property we will learn about is the `config` object.

This object may contain various properties that will change how our subnet is identified and behaves.

Inside the `config` object we will add the `chainId` property.

```
{
  "config: {
    "chainId": 1234
  }
}
```

The `chainId` is how your subnet will be identified. It must be unique and cannot collide with any other blockchain. For example main net Ethereum uses `chainId: 1`. You can use [ChainList](https://chainlist.org/) to ensure you pick a chain ID that is not already taken. 

The next set of properties we will learn about are called hardfork activation times.

```
{
  "config: {
    "chainId": 1234,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip150Hash": "0x2086799aeebeae135c246c65021c82b4e15a2c451340993aacfd2751886514f0",
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "muirGlacierBlock": 0,
    "subnetEVMTimestamp": 0,
  }
}
```
These eleven properties determine when each hardfork is activated. Changing these can cause issues so only do so if you have a specific usecase in mind and be sure you know what you're doing.

Now we will move onto the `feeConfig` object

```
{
  "config: {
    "chainId": 1234,
    "homesteadBlock": 0,
    "eip150Block": 0,
    "eip150Hash": "0x2086799aeebeae135c246c65021c82b4e15a2c451340993aacfd2751886514f0",
    "eip155Block": 0,
    "eip158Block": 0,
    "byzantiumBlock": 0,
    "constantinopleBlock": 0,
    "petersburgBlock": 0,
    "istanbulBlock": 0,
    "muirGlacierBlock": 0,
    "subnetEVMTimestamp": 0,
    "feeConfig": {
      "gasLimit": 10000000,
      "minBaseFee": 25000000000
      "targetGas": 15000000,
      "baseFeeChangeDenominator": 36,

    }
  }
}
```

This object has a number of useful properties that can be used to customize the fees of your subnet.

### gasLimit

The `gasLimit` is the maximum amount of gas that can be used by all the transactions in a block. This is a fixed value and it will not change based of congestion so it should be chosen carefully. A higher gas limit will allow for more transactions or for more computationally heavy transaction to be included in each block.

### minBaseFee

The `minBaseFee` is used as the minimum fee of transactions as well as the initial base fee for EIP-1559 blocks. This is tracked in units of NanoAvax `1 Avax == 1000000000 NanoAvax`. This means that every transaction that is included in a block on your subnet will need to pay at least the `minBaseFee`. Increasing this will mean that it will be more expensive to do any transaction, no matter how small.

### targetGas

The `targetGas` property is the gas target that each block aims to consume. If blocks start to consume more than this amount of gas, then the base fees will be increased.

### baseFeeChangeDenominator

The `baseFeeChangeDenominator` is how much the base fee can be changed between blocks