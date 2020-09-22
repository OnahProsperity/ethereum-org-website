---
title: ERC-20 Token Standard
description:
lang: en
sidebar: true
---

## Introduction

**What is a Token?**

Tokens can virtually represent anything in Ethereum, from reputation points in an online platform, skills of a character
in a game and lottery tickets till financial asset bond to a real good like a share in a company, a fiat currency, a
gold ounce and so on! Such a powerful feature deserves and must be handled by a robust standard, right? That's exactly
where the ERC-20 plays its role!

**What is ERC-20?**

The ERC-20 introduces a standard for Fungible Tokens, in other words, they have a property that makes each Token be exactly
the same (in type and value) of another Token. For example, an ERC-20 Token acts just like the ETH, meaning that 1 Token
is and will always be equal to all the other Tokens.

## Prerequisites

- [Accounts](/developers/docs/accounts)
- [Smart Contracts](/developers/docs/smart-contracts/)
- [Token standards](/developers/docs/standards/tokens/)

## Body

The ERC-20 (Ethereum Request for Comments 20), proposed by Fabian Vogelsteller in November 2015, is a Token Standard that
implements an API for tokens within Smart Contracts.

It provides functionalities like to transfer tokens from one account to another, to get the current token balance of an
account and also the total supply of the token available on the network. Besides these it also has some other functionalities
like to approve that an amount of token from an account can be spent by a third party account.

If a Smart Contract implements the following methods and events it can be called an ERC-20 Token Contract and, once deployed, it
will be responsible to keep track of the created tokens on Ethereum.

From [EIP-20](https://eips.ethereum.org/EIPS/eip-20):

#### Methods:

```solidity
# Returns the name of the token - e.g. "MyToken".
function name() public view returns (string)
```

```solidity
# Returns the symbol of the token. E.g. “HIX”.
function symbol() public view returns (string)
```

```solidity
# Returns the number of decimals the token uses - e.g. 8, means to divide the token amount by 100000000 to get its user representation.
function decimals() public view returns (uint8)
```

```solidity
# Returns the total token supply.
function totalSupply() public view returns (uint256)
```

```solidity
# Returns the account balance of another account with address _owner.
function balanceOf(address _owner) public view returns (uint256 balance)
```

```solidity
# Transfers _value amount of tokens to address _to.
function transfer(address _to, uint256 _value) public returns (bool success)
```

```solidity
# Transfers _value amount of tokens from address _from to address _to
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
```

```solidity
# Allows _spender to withdraw from your account multiple times, up to the _value amount.
function approve(address _spender, uint256 _value) public returns (bool success)
```

```solidity
# Returns the amount which _spender is still allowed to withdraw from _owner.
function allowance(address _owner, address _spender) public view returns (uint256 remaining)
```

#### Events:

```solidity
# MUST trigger when tokens are transferred, including zero value transfers.
event Transfer(address indexed _from, address indexed _to, uint256 _value)
```

```solidity
# MUST trigger on any successful call to approve().
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

### Examples

Let's see how a Standard is so important to make things simple for us to inspect any ERC-20 Token Contract on Ethereum.
We just need the Contract Application Binary Interface (ABI) to instantiate an interface to any ERC-20 Token. As you can
see below we will use a simplified ABI, to make it a low friction example.

#### Web3.py Example

First, make sure you have installed [Web3.py](https://web3py.readthedocs.io/en/stable/quickstart.html#installation) Python library:

```
$ pip install web3
```

```python
from web3 import Web3


w3 = Web3(Web3.HTTPProvider("https://cloudflare-eth.com"))

dai_token_addr = "0x6B175474E89094C44Da98b954EedeAC495271d0F"     # DAI
weth_token_addr = "0xC02aaA39b223FE8D0A0e5C4F27eAD9083C756Cc2"    # Wrapped Ether (WETH)

acc_address = "0xA478c2975Ab1Ea89e8196811F51A7B7Ade33eB11"        # Uniswap V2: DAI 2

# This is a simplified Contract Application Binary Interface (ABI) of an ERC-20 Token Contract.
# It will expose only the methods: balanceOf(address), decimals(), symbol() and totalSupply()
simplified_abi = [
    {
        'inputs': [{'internalType': 'address', 'name': 'account', 'type': 'address'}],
        'name': 'balanceOf',
        'outputs': [{'internalType': 'uint256', 'name': '', 'type': 'uint256'}],
        'stateMutability': 'view', 'type': 'function', 'constant': True
    },
    {
        'inputs': [],
        'name': 'decimals',
        'outputs': [{'internalType': 'uint8', 'name': '', 'type': 'uint8'}],
        'stateMutability': 'view', 'type': 'function', 'constant': True
    },
    {
        'inputs': [],
        'name': 'symbol',
        'outputs': [{'internalType': 'string', 'name': '', 'type': 'string'}],
        'stateMutability': 'view', 'type': 'function', 'constant': True
    },
    {
        'inputs': [],
        'name': 'totalSupply',
        'outputs': [{'internalType': 'uint256', 'name': '', 'type': 'uint256'}],
        'stateMutability': 'view', 'type': 'function', 'constant': True
    }
]

dai_contract = w3.eth.contract(address=w3.toChecksumAddress(dai_token_addr), abi=simplified_abi)
symbol = dai_contract.functions.symbol().call()
decimals = dai_contract.functions.decimals().call()
totalSupply = dai_contract.functions.totalSupply().call() / 10**decimals
addr_balance = dai_contract.functions.balanceOf(acc_address).call() / 10**decimals

#  DAI
print("===== %s =====" % symbol)
print("Total Supply:", totalSupply)
print("Addr Balance:", addr_balance)

weth_contract = w3.eth.contract(address=w3.toChecksumAddress(weth_token_addr), abi=simplified_abi)
symbol = weth_contract.functions.symbol().call()
decimals = weth_contract.functions.decimals().call()
totalSupply = weth_contract.functions.totalSupply().call() / 10**decimals
addr_balance = weth_contract.functions.balanceOf(acc_address).call() / 10**decimals

#  WETH
print("===== %s =====" % symbol)
print("Total Supply:", totalSupply)
print("Addr Balance:", addr_balance)
```

## Further reading

- [EIP-20: ERC-20 Token Standard](https://eips.ethereum.org/EIPS/eip-20)
- [OpenZeppelin - Tokens](https://docs.openzeppelin.com/contracts/3.x/tokens#ERC20)
- [OpenZeppelin - ERC-20 Implementation](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)
- [ConsenSys - ERC-20 Implementation](https://github.com/ConsenSys/Tokens/blob/master/contracts/eip20/EIP20.sol)

## Related topics

- [ERC-721](/developers/docs/standards/tokens/erc-721/)
- [ERC-777](/developers/docs/standards/tokens/erc-777/)
- [ERC-1155](/developers/docs/standards/tokens/erc-1155/)