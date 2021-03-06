### Summary:

This implementation of tokens are based on Open Zeppelin Standard Token ERC-20 contract. This contract has been further mixed in to include "pausable" and "ownable" traits.

>Based  Project Used for this demo:
>https://erc20token.sonnguyen.ws/en/latest/erc20_token/


| Reference| Source |
| ------------------ | ---- |
| ERC-20 Contract| https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/token/ERC20/ERC20.sol |
| Pausable Contract| https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/lifecycle/Pausable.sol |
| Ownable Contract| https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/ownership/Ownable.sol |
| SafeMath Library |  https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/math/SafeMath.sol |


**Total Supply**

1 ETH = 1000 Token [ you can change this ratio by making chances in migrations/2_deploy_contracts.js file ]

### Requirement:

- Nodejs v9.10 or above
- Truffle v4.1.8 (core: 4.1.8) (http://truffleframework.com/docs/getting_started/installation)
- Solidity v0.4.23
- Metamask (https://metamask.io) /Ganache Wallet

> [Please Note : infura.io provider is used for the demo ]

### Deployment Steps:
```
git clone https://github.com/imperialsoftech/okt-token
cd okt-token/
npm install
```
**Update the migrations/2_deploy.contracts.js file**
```
const IcoToken = artifacts.require('IcoToken');
const IcoContract = artifacts.require('IcoContract');

module.exports = function(deployer){
	deployer.deploy(
			IcoToken,
			'OK Token',
			'OKT',
			'18',
			'1.0'
		).then(() => {
			return deployer.deploy(
					IcoContract,
					'0xab0874cB6........', // put your Rinkeby wallet address here, for link your account to token contract
					IcoToken.address,
					'100000000000000000000000000', // 100000000 Token
				    '1000', // 1 ETH = 1000 Token
				    '1514764800', // 01/01/2018
				    '1546214400', // 31/12/2018
				    '100000000000000000' // 0.1 ETH
				).then(() => {
					return IcoToken.deployed().then(function(instance){
						return instance.setIcoContract(IcoContract.address);	
					});
				});
		});
};

```

Create an account at infura.io and update your key with API_KEY_HERE

**Update the truffle.js file**
```
var HDWalletProvider = require("truffle-hdwallet-provider");
module.exports = 
{
    networks: 
    {
	    rinkeby: {
		    provider: function() {
		      var mnemonic = 'steel neither flight ...'; //put ETH wallet 12 mnemonic code	
		      return new HDWalletProvider(mnemonic, "https://rinkeby.infura.io/<API_KEY_HERE>"); // 
		    },
		    network_id: '4',
		}  
    }
};
```
Also update your ethereum address and end date timestamp in migrations/2_deploy_contracts.js file 

**Go to your project folder in terminal then execute :**

```
truffle compile 
truffle migrate --network rinkeby
```

Send 1 ETH to ICOContract address from your rinkeby wallet

**To View You Deployed Contract on Rinkeby Etherscan :** 

>https://rinkeby.etherscan.io/token/<ICOToken_Contract_Address>


`Our Deployed Contracts on Rinkeby Network:`

**ICOToken :** 
> https://rinkeby.etherscan.io/token/0xae7f5205299f37191e8ef9f81539ef3e0ec7f455

**ICOContract :** 
> https://rinkeby.etherscan.io/address/0xdb8c2257d8c6964b1a0d33bdbb9ffe3f5c23b4a2