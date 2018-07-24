---
title: '[Blog] The Complete Tutorial On Getting Started With Smart Contracts On Ethereum'
date: 2018-07-12 12:32:51
tags:
- Case Studies
- Projects
- Audits
category: "Projects"
author: "Rico Chen"
thumbnail: https://i.imgur.com/dDZSHJ8.png
---

### The Complete Tutorial On Getting Started With Smart Contracts On Ethereum

![img](https://cdn-images-1.medium.com/max/1000/1*NvpErISrR0LNhU61PGh2eQ.png)

> Every download, install, framework, tool, and process you will need to get started. This tutorial assumes you have an understanding of Ethereum, Smart Contracts, and how to code.

---

#### Download and install Node.js

Go to <https://nodejs.org/en/> and download the version for Latest Features on the right. After it’s done downloading, install it.

Verify you have node and npm installed:

```
node -v
// v10.1.0
```

```
npm -v
// v6.0.1
```

---

#### Download and install an IDE

You can use any text editor or IDE but the recommended ones for Solidity are Visual Studio Code and Atom. I use VS Code.

Go to <https://code.visualstudio.com/> and download Stable Build. After it’s done downloading, open it.

Click the last icon on the left side bar to open Extensions. Search for _solidity_ and choose the one by Juan Blanco. Install it. Reload VS Code to use the extension.

![img](https://cdn-images-1.medium.com/max/1000/1*XkRDU7ba6Q8bjYZYV9v96w.png)

Here are my VS Code preferences, go to File > Preferences > Settings and copy the below into User Settings:

```
{
    "editor.dragAndDrop": false,
    "editor.fontSize": 12,
    "editor.formatOnSave": true,
    "editor.formatOnType": true,
    "editor.formatOnPaste": true,
    "editor.wordWrap": "on",
    "editor.quickSuggestions": {
        "other": true,
        "comments": true,
        "strings": true
    },
    "explorer.confirmDragAndDrop": false,
    "explorer.confirmDelete": false,
    "files.autoSave": "onFocusChange",
    "workbench.colorTheme": "Default Light+",
    "solidity.enabledAsYouTypeCompilationErrorCheck": true,
    "solidity.linter": "solium",
    "solidity.packageDefaultDependenciesContractsDirectory": "",
    "solidity.packageDefaultDependenciesDirectory": "",
    "solidity.validationDelay": 1500
}
```

If you’re using Windows, skip to **Install Truffle**. If you’re using Mac, follow this step to add VS Code to your PATH, which allows launching VS Code from the command line.

Open VS Code. Open the Command Palette (⇧⌘P), type _shell command_ and click Shell Command: Install ‘code’ command in PATH. Now you can open a directory using `code .`

---

#### Install Truffle

Truffle is the most popular development framework for Ethereum.

```
npm install -g truffle
```

Verify you have truffle installed:

```
truffle v
// Truffle v4.1.8 (core: 4.1.8)
// Solidity v0.4.23 (solc-js)
```

---

#### Truffle Boxes

**Truffle Boxes** are helpful boilerplates that should be taken advantage of instead of starting from scratch, such as a react app or a coin example. <http://truffleframework.com/boxes/>

1.  Create a new directory for your Truffle project:

```
mkdir MetaCoin
cd MetaCoin
```

\2. Download (“unbox”) the MetaCoin box:

```
truffle unbox metacoin
```

\3. Open the directory:

```
code .
```

Now you should have a project structure with the following items:

- `contracts/`: Directory for [Solidity contracts](http://truffleframework.com/docs/getting_started/contracts)
- `migrations/`: Directory for [scriptable deployment files](http://truffleframework.com/docs/getting_started/migrations#migration-files)
- `test/`: Directory for test files for [testing your application and contracts](http://truffleframework.com/docs/getting_started/testing)
- `truffle.js`: Truffle [configuration file](http://truffleframework.com/docs/advanced/configuration)

---

#### Using Truffle and Open Zeppelin to create an ERC20 Token

Instead of using a Truffle box we can also initialize an empty Truffle project. Here is a link to the repo: <https://github.com/ricochen/ERC20Token>

Let’s leave the MetaCoin directory and create a new one:

```
cd ..
mkdir ERC20Token
cd ERC20Token
```

Initialize the Truffle project: (directory MUST be empty)

```
truffle init
```

Open the directory:

```
code .
```

You should see a similar directory structure as the MetaCoin box. `truffle init` creates the skeleton and necessary files for a Truffle project.

You will see that `Migrations.sol` has errors, this is because of the Solidity extension we installed. `Migrations.sol` is using outdated syntax compared to our linter rules, fix them by using 4 indentations instead of 2, and changing

```
function Migrations() public {
```

```
  owner = msg.sender;
```

```
}
```

to

```
constructor() public {
```

```
    owner = msg.sender;
```

```
}
```

---

**Open Zeppelin**

OpenZeppelin is a library for writing secure Smart Contracts on Ethereum. <https://github.com/OpenZeppelin/openzeppelin-solidity>. OpenZeppelin is meant to provide secure, tested and community-audited code, however, nothing is 100% secure so make sure to thoroughly test and audit your code. Take a look in their contracts folder and become familiar with the code as they have become a standard for writing Smart Contracts. We will be using their implementation of an ERC20 Token.

To install the OpenZeppelin library, run the following in your Solidity project root directory:

```
npm init -y
npm install -E openzeppelin-solidity
```

Coding a Smart Contract:

Create a new file under `/contracts` and name it `ERC20Token.sol` , the .sol means it’s a Solidity file. Write the Solidity version we will be using at the top:

```
pragma solidity ^0.4.23;
```

Import the Open Zeppelin dependencies for our ERC20 Token:

```
import "openzeppelin-solidity/contracts/token/ERC20/StandardToken.sol";
```

You will see an error with the import. Currently, the only way to make the Open Zeppelin import work with our Solidity extension is to update our preferences. This will likely be fixed in the future. In VS Code, type `ctrl + comma` to open User Settings and add this and save:

```
"solidity.packageDefaultDependenciesContractsDirectory": "",
"solidity.packageDefaultDependenciesDirectory": "",
```

Write the ERC20 Token contract:

```
/**
 * @title ERC20Token
 * @dev Very simple ERC20 Token example, where all tokens are pre-assigned to the creator.
 * Note they can later distribute these tokens as they wish using `transfer` and other
 * `StandardToken` functions.
 */
contract ERC20Token is StandardToken {
```

```
    string public constant name = "ERC20Token"; // solium-disable-line uppercase
    string public constant symbol = "ERC"; // solium-disable-line uppercase
    uint8 public constant decimals = 18; // solium-disable-line uppercase
```

```
    uint256 public constant INITIAL_SUPPLY = 10000 * (10 ** uint256(decimals));
```

```
    /**
     * @dev Constructor that gives msg.sender all of existing tokens.
     */
    constructor() public {
        totalSupply_ = INITIAL_SUPPLY;
        balances[msg.sender] = INITIAL_SUPPLY;
        emit Transfer(0x0, msg.sender, INITIAL_SUPPLY);
    }
}
```

We’re going to make it a simple ERC20 Token which includes a name, symbol, decimal, and initial supply. <https://github.com/OpenZeppelin/openzeppelin-solidity/blob/master/contracts/examples/SimpleToken.sol>.

Writing Tests

Next we’re going to write tests for our ERC20 Token. Create a new file under `/test` and name it `ERC20Token.test.js` :

```
const ERC20Token = artifacts.require('ERC20Token');
```

```
contract('ERC20Token', accounts => {
  let token;
  const creator = accounts[0];
```

```
beforeEach(async function () {
    token = await ERC20Token.new({ from: creator });
  });
```

```
it('has a name', async function () {
    const name = await token.name();
    assert.equal(name, 'ERC20Token');
  });
```

```
it('has a symbol', async function () {
    const symbol = await token.symbol();
    assert.equal(symbol, 'ERC');
  });
```

```
it('has 18 decimals', async function () {
    const decimals = await token.decimals();
    assert(decimals.eq(18));
  });
```

```
it('assigns the initial total supply to the creator', async function () {
    const totalSupply = await token.totalSupply();
    const creatorBalance = await token.balanceOf(creator);
```

```
    assert(creatorBalance.eq(totalSupply));
  });
});
```

Run the test:

```
truffle test
```

You will see the following output:

```
Contract: ERC20Token
    ✓ has a name
    ✓ has a symbol
    ✓ has 18 decimals
    ✓ assigns the initial total supply to the creator
```

```
4 passing (356ms)
```

Additional files

Create a new file in the root directory and name it `.gitignore`. We will use it to ignore our `/build` and `/node_modules` folders when committing to GitHub.

```
build/
node_modules/
```

Create a new file in the root directory and name it `.gitattributes`. This will be used to add syntax color to our .sol files on GitHub.

```
*.sol linguist-language=Solidity
```

---

#### Deploying to Ganache (Local)

Now that we have the contract and test finished, we will migrate and deploy the contract using Ganache, a personal blockchain for Ethereum development that runs on your desktop. Ganache has a UI that you can download and interact with, as well as a command line option. We will be using the latter, ganache-cli. <https://github.com/trufflesuite/ganache-cli/>

```
npm install -g ganache-cli
```

You may have noticed there are two truffle config files. The reason is that in Windows there is a naming conflict with `truffle.js`. To fix this we can simply delete `truffle.js` and use `truffle.config.js` which will work for both Windows and Mac.

Add the ganache-cli network to our `truffle-config.js` file:

```
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    }
  }
};
```

Run ganache-cli:

```
ganache-cli
```

Ganache will generate 10 test accounts which each have 100 ether by default, and are unlocked for us to use.

Open another command line and go to our ERC20 Token directory and compile the contracts:

```
truffle compile
```

You will see the following output:

```
Compiling ./contracts/ERC20Token.sol...
Compiling ./contracts/Migrations.sol...
Compiling openzeppelin-solidity/contracts/math/SafeMath.sol...
Compiling openzeppelin-solidity/contracts/token/ERC20/BasicToken.sol...
Compiling openzeppelin-solidity/contracts/token/ERC20/ERC20.sol...
Compiling openzeppelin-solidity/contracts/token/ERC20/ERC20Basic.sol...
Compiling openzeppelin-solidity/contracts/token/ERC20/StandardToken.sol...
Writing artifacts to ./build/contracts
```

You will see a `/build` folder that contains our compiled contracts in JSON.

We will need a migrations script to help us deploy our ERC20 Token contract to the blockchain. Create a new file under `/migrations` and name it `2_deploy_contracts.js`:

```
var ERC20Token = artifacts.require("./ERC20Token.sol");
```

```
module.exports = function(deployer) {
  deployer.deploy(ERC20Token);
};
```

Migrate and deploy the contracts to our development blockchain:

```
truffle migrate --network development
```

You will see the following output:

```
Using network 'development'.
```

```
Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x4f87ddd3c299752294356431998d511e1879b08d3a6b5acff36d4727f92690e0
  Migrations: 0x90678eeba892762d1d456ae58872c94eba2c2e52
Saving successful migration to network...
  ... 0x09bbbf050c015ed94a48a92f842605b9f2f857a18cc2c90bd6c9b5b37b630aa1
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying ERC20Token...
  ... 0x94e4617012532bcd90a9f92ef349fd5990430596dd4e1c2733c543ebb1d35c0b
  ERC20Token: 0x395892c139c59533941c87a6dad33e2889fd5de3
Saving successful migration to network...
  ... 0x06dc8833243d8fb100c8bd2117ad246721488273eac79bab40a4c70437262dc2
Saving artifacts...
```

Run the following to start Truffle’s console, which will allow us to interact and test the newly deployed ERC20 Token.

```
truffle console --network development
```

Store the ERC20 Token instance in a variable:

```
ERC20Token.deployed().then(inst => ERC20TokenInstance = inst)
```

Check the ERC20 Token instance name and total supply:

```
ERC20TokenInstance.name()
```

```
// 'ERC20Token'
```

```
ERC20TokenInstance.totalSupply().then(function(i) {totalSupply = i})
```

```
// undefined
```

```
totalSupply.toNumber()
```

```
// 1e+22
```

---

#### Deploying to Rinkeby (Testnet)

Geth is the command line interface for running a full ethereum node implemented in Go. We will use Geth to deploy to the Rinkeby Testnet.

For Windows:

Go to <https://geth.ethereum.org/downloads/> to download and install Geth. Check Geth and Development tools.

For Mac:

Install Homebrew:

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Install Geth:

```
brew tap ethereum/ethereum
brew install ethereum
```

Check Geth is installed:

```
geth version
```

```
// Geth
Version: 1.8.8-stable
Architecture: amd64
Protocol Versions: [63 62]
Network Id: 1
Go Version: go1.10.2
Operating System: darwin
GOPATH=
GOROOT=/usr/local/Cellar/go/1.10.2/libexec
```

Create a directory to store your blockchain files:

```
mkdir rinkeby
cd rinkeby
```

Initialize the genesis block:

```
curl -O https://www.rinkeby.io/rinkeby.json
geth --datadir=. init rinkeby.json
```

Download the Rinkeby blockchain:

```
geth --datadir=. --rpc --rinkeby --rpcapi "web3,eth,net,personal" --cache=1024
```

You will see the follow output:

```
INFO [05-14|21:08:37] Maximum peer count                       ETH=25 LES=0 total=25
INFO [05-14|21:08:37] Starting peer-to-peer node               instance=Geth/v1.8.7-stable-66432f38/windows-amd64/go1.10.1
INFO [05-14|21:08:37] Allocated cache and file handles         database=C:\\Users\\ricox\\Documents\\GitHub\\projects\\rinkeby\\geth\\chaindata cache=768 handles=1024
INFO [05-14|21:08:37] Persisted trie from memory database      nodes=355 size=65.27kB time=467.9µs gcnodes=0 gcsize=0.00B gctime=0s livenodes=1 livesize=0.00B
INFO [05-14|21:08:37] Initialised chain configuration          config="{ChainID: 4 Homestead: 1 DAO: <nil> DAOSupport: true EIP150: 2 EIP155: 3 EIP158: 3 Byzantium: 1035301 Constantinople: <nil> Engine: clique}"
INFO [05-14|21:08:37] Initialising Ethereum protocol           versions="[63 62]" network=4
WARN [05-14|21:08:37] Head state missing, repairing chain      number=2263453 hash=24f662…e2ea0b
INFO [05-14|21:08:37] Rewound blockchain to past state         number=2263053 hash=33b947…61815f
INFO [05-14|21:08:37] Loaded most recent local header          number=2263453 hash=24f662…e2ea0b td=4237648
INFO [05-14|21:08:37] Loaded most recent local full block      number=2263053 hash=33b947…61815f td=4236848
INFO [05-14|21:08:37] Loaded most recent local fast block      number=2263453 hash=24f662…e2ea0b td=4237648
INFO [05-14|21:08:37] Loaded local transaction journal         transactions=6 dropped=0
INFO [05-14|21:08:37] Regenerated local transaction journal    transactions=6 accounts=1
WARN [05-14|21:08:37] Blockchain not empty, fast sync disabled
INFO [05-14|21:08:38] Starting P2P networking
INFO [05-14|21:08:40] UDP listener up                          self=enode://fdad6fb872dc90d587b0225a5b7f3c7d2d53efef6574415a1cf8f0c42da0791b80553a1753403d2c118b33a2b9cb474673e9c759394a92f1c0435edbaddf756f@76.102.174.73:30303
INFO [05-14|21:08:40] RLPx listener up                         self=enode://fdad6fb872dc90d587b0225a5b7f3c7d2d53efef6574415a1cf8f0c42da0791b80553a1753403d2c118b33a2b9cb474673e9c759394a92f1c0435edbaddf756f@76.102.174.73:30303
INFO [05-14|21:08:40] IPC endpoint opened                      url=\\\\.\\pipe\\geth.ipc
INFO [05-14|21:08:40] HTTP endpoint opened                     url=http://127.0.0.1:8545 cors= vhosts=localhost
```

To check the blockchain sync process, copy the IPC endpoint in bold (mine is `\\\\.\\pipe\\geth.ipc`, yours may be different). Open another command line and paste your IPC endpoint after `geth attach`.

```
geth attach \\\\.\\pipe\\geth.ipc
```

You will see the following output:

```
Welcome to the Geth JavaScript console!
```

```
instance: Geth/v1.8.7-stable-66432f38/windows-amd64/go1.10.1
 modules: admin:1.0 clique:1.0 debug:1.0 eth:1.0 miner:1.0 net:1.0 personal:1.0 rpc:1.0 txpool:1.0 web3:1.0
```

Type `eth.syncing` to check the progress, this can take a few hours:

```
eth.syncing
```

```
// {
  currentBlock: 35191,
  highestBlock: 2290216,
  knownStates: 68389,
  pulledStates: 52406,
  startingBlock: 0
}
```

Wait until the syncing is finished before moving on, it will output `false`:

```
eth.syncing
```

```
// false
```

Open another command line and go to your Rinkeby directory. Create a new account to use for Rinkeby, it will be stored in `/keystore`:

```
cd rinkeby
geth --datadir=. account new
```

Type in your password and repeat it, then you should receive your address:

```
Your new account is locked with a password. Please give a password. Do not forget this password.
Passphrase:
Repeat passphrase:
Address: {f6e9dc98fe5d2951744967133b3c31765be657c1}
```

To check your accounts, go to the Geth JavaScript console tab and type:

```
eth.accounts
// ["0xf6e9dc98fe5d2951744967133b3c31765be657c1"]
```

```
eth.coinbase
// "0xf6e9dc98fe5d2951744967133b3c31765be657c1"
```

To check your balance:

```
eth.getBalance(eth.coinbase)
// 0
```

We’re going to need ETH to make transactions in Rinkeby. To request ETH from the Rinkeby faucet, we need to make a public post using Twitter, Facebook, or Google+.

I will use Google+, go to <https://plus.google.com/> and make a public post with your address with the 0x prepended and without the quotes. Click the arrow to get the url such as: <https://plus.google.com/105101964713106743199/posts/LC5VqAxZrcd>

Copy this url and paste it into <https://www.rinkeby.io/#faucet> and click _Give me Ether_ and choose _3 Ethers / 8 hours_. You will have to wait 8 hours to request again using Google+. If the transaction was successful but something went wrong, use Twitter or Facebook using the same steps above so you don’t have to wait.

![img](https://cdn-images-1.medium.com/max/1000/1*d68lMQH5rYybj3lIH3YuLA.png)

Wait one minute for the transaction to finish and verify your transaction with your Rinkeby address on Etherscan:
<https://rinkeby.etherscan.io/address/0xf6e9dc98fe5d2951744967133b3c31765be657c1>

![img](https://cdn-images-1.medium.com/max/1000/1*Dj-p5EwkaInTSRcbdzTIww.png)

Also verify through the Geth JavaScript console:

```
eth.getBalance(eth.coinbase)
// 3000000000000000000
```

Unlock your account, type your password and press enter:

```
personal.unlockAccount(eth.coinbase)
// Unlock account 0xf6e9dc98fe5d2951744967133b3c31765be657c1
// Passphrase:
// true
```

Open a new command line and open your ERC20 Token directory:

```
cd ERC20Token
code .
```

Add the Rinkeby network to your `truffle-config.js` file and replace _from_ with your address:

```
module.exports = {
  networks: {
    development: {
      host: "localhost",
      port: 8545,
      network_id: "*" // Match any network id
    },
    rinkeby: {
      host: "localhost",
      port: 8545,
      network_id: "4", // Rinkeby ID 4
      from: "0xf6e9dc98fe5d2951744967133b3c31765be657c1" // account from which to deploy
    }
  }
};
```

Migrate and deploy the contracts to Rinkeby Testnet:

```
truffle migrate --network rinkeby
```

You will see the following output:

```
Using network 'rinkeby'.
```

```
Running migration: 1_initial_migration.js
  Deploying Migrations...
  ... 0x657f08c640c5427b5c6ceba4b0a357a990540b117173ec8152d7c95ce1ae01ed
  Migrations: 0xd1ac4c1f8695ff626ddc172080ebfa3c68b0ade9
Saving successful migration to network...
  ... 0xc4805b117afcb4541722b3428966f14389166bac7c39e3909f544c1634ee8da6
Saving artifacts...
Running migration: 2_deploy_contracts.js
  Deploying ERC20Token...
  ... 0x13da61f42445b7809700de645232e514b7757154ba030b4a962516c72d3716e9
  ERC20Token: 0x17db24765fc009a5b42360ef500daf839f51a040
Saving successful migration to network...
  ... 0x8af12b2773de916acf8be3ec67a3ec4c1d0cb9a493ac675cc0910eef50fd6e52
Saving artifacts...
```

Wait a minute and verify it’s deployed on Etherscan: <https://rinkeby.etherscan.io/address/0xf6e9dc98fe5d2951744967133b3c31765be657c1>

![img](https://cdn-images-1.medium.com/max/1000/1*tW6E85hD_VpSGDpVTWcJEA.png)

For future uses with Rinkeby, save these commands:

Open one command line for Geth, replace with your address and type in your password after:

```
geth --datadir ./rinkeby --rpc --rinkeby --rpcapi "web3,eth,net,personal" --cache=1024 --unlock "0xacfa76dbd4a46d5497f8d657cfc8a343c69b584d"
```

```
// Unlocking account 0xacfa76dbd4a46d5497f8d657cfc8a343c69b584d | Attempt 1/3
Passphrase:
```

Open a second command line for the Geth JavaScript console, replace with your IPC endpoint:

```
geth attach \\\\.\\pipe\\geth.ipc
eth.syncing
```

Open a third command line for truffle:

```
truffle migrate --network rinkeby
truffle console --network rinkeby
```

```
ERC20Token.deployed().then(inst => ERC20TokenInstance = inst)
ERC20TokenInstance.name()
```
