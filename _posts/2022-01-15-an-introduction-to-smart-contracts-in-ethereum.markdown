
As a precursor to my in-progress **NFT Minting** article, I've gone ahead and wrote an intro to smart contract programming in Ethereum. I've included a TOC/Outline to jump around below. This is a technical topic.
<div markdown="1" align="center"> 

![](/assets/smartcontracts.jpeg) 

</div>
<h3 id="heading--outline">Outline</h3>

* [Ethereum Nodes](#heading--nodes)
* [Smart Contract Development](#heading--scd)
* [Solidity, Ethereum's smart contract programming language](#heading--solidity)
* [Deploying a smart contract via Hardhat](#heading--hardhatdeploy)
* [Reading and writing with our smart contract](#heading--readwrite)

<h2 id="heading--nodes">Ethereum Nodes</h2>

An Ethereum node is necessary in order to interact with the Ethereum blockchain. Generally, the node software for any blockchain will be the main entrypoint into deciphering blockchain data. Usually you would spend hours or days downloading the entire blockchain before you could do anything useful with it ( indeed, the Ethereum blockchain is now over 1TB ), but in these modern times, with the software-as-a-service (SaaS) model in full force, somebody has made a web application that simplifies that process. Basically, when you sign up for one of these services ( [Infura](https://infura.io/), [AlchemyAPI](https://alchemyapi.io/) ) you are given API keys to access their Ethereum node cluster, which is distributed across the globe. You have in essence a 'supernode' at your disposal and for business applications it is a no-brainer, saving time and ultimate money in the form of storage costs. There is a very useful article on the Ethereum Developer Docs website about this if anyone is interested in learning more: [Nodes-as-a-service](https://ethereum.org/en/developers/docs/nodes-and-clients/nodes-as-a-service/). We will be using **AlchemyAPI** for the rest of the post.

After signing up for AlchemyAPI, you create a new application and are assigned an API key for you to use in your application. I should mention that I use **Linux** in this post and if you are unfamiliar with the command-line I urge you to leave a comment wherever confusion may occur. In Alchemy our API key looks something like `mlXg7QF2KW57UICIafHBZSqlt7-uCaUK`, a string of meaningless symbols.

<h1 id="heading--scd">Smart Contract Development</h1>

Developing our first smart contract used to be a fairly convoluted affair, needing extensive use of the command line and compilers that were build from the core Ethereum codebase, essentially low-level tools. Thankfully, in this modern age, we have the ability to pull in something called a *smart contract framework*, which will do the heavy lifting of compilation, testing and other gruntwork-type tasks. There are several frameworks for Ethereum development available, such as [Hardhat](https://hardhat.org) or [Truffle](https://www.trufflesuite.com), and we will be using one to develop our NFT contract. In this post we will be using **Hardhat**, although Truffle is equivalent in terms of features.

You will need **node.js** v12 or greater. If you are unfamiliar with how to install node, you can follow the [Hardhat tutorial](https://hardhat.org/tutorial/setting-up-the-environment.html). Run or read the commands in blockquotes to understand roughly what is being done.
```
mkdir scd-walkthrough
cd scd-walkthrough
npm init --yes 
npm install --save-dev hardhat 
```
Running the above will initalize a new Hardhat project. Create an empty Hardhat config using `npx hardhat`.

```
mkdir contracts
mkdir scripts
```
We will use these directories to organize smart contracts (contracts/) and scripts to launch them (scripts/). Next we write some smart contract code, finally!

<div markdown="1" align=center>

![](/assets/SolGray.png) 
</div>

<h2 id="heading--solidity">Solidity, Ethereum's smart contract programming language</h2>

Create a file called `hello.sol` in the contracts/ directory and paste the following code.

    pragma solidity ^0.7.3;
   
    contract HelloWorld {
       string public message;

       constructor(string memory initMessage) {
          message = initMessage;
       }

       function update(string memory newMessage) public {
          message = newMessage;
       }
    }

You can see this very small contract has a structure, and if you are familiar with Python it may feel vaguely similar. The top line of a Solidity contract is the last version number of the Solidity compiler that will correctly compile the contract. In this example, we need a compiler of at least version 0.7.3.

The next line `contract HelloWorld` is the definition of a new contract, similar to the declaration of a new class in Java. The brackets indicate the code block for the contract, and are required. `string public message` declares a new contract-scoped variable called `message` that is of type `string`. The annotation `public` implies the variable is accessible by other contracts. A getter method is automatically generated by the compiler when using the keyword `public` of the same name as the variable, eg. `HelloWorld HW = new HelloWorld(""); HW.message();` would retrieve the contents of the string.

The following line, `constructor(string memory initMessage)` is a constructor function, that is used when instantiating the contract, similarly to Java, where you would call the constructor when instantiating a Java class. This contract's contstructor takes a `string` as a parameter. **Solidity is a statically typed language**, and depending on the type, the variable is passed by value (copied) or reference (pointer). The `string` type is a reference type and, due to this, needs an additional annotation `memory` to specify the lifetime of the variable, presumably for garbage collection. `memory` implies the variable will live for the lifetime of the immediate function call, `storage` the lifetime of the contract, and `calldata` a special data location with immutable properties. Our `initMessage` variable then is declared in the constructor for the life of the constructor call, then cleaned up. 

Inside the constructor we see `message = initMessage` where we assign the constructor string variable to the contract variable for use by contract functions.

`function update(string memory newMessage) public` here we see functions are declared with the `function` keyword, followed by the name of the function, its parameters and modifier `public`, which allows external contracts to call this function. Other modifiers for functions include `pure`, which promise not to read or modify state, and `view` which promises not to modify state, and `fallback` which is a special function that is called when no data is supplied or the caller requested a function that is unavailable.

Finally, we see `message = newMessage` in the last non-bracketed line, which clearly takes the contract's string and replaces it with the one from a call to `update()`. In general Solidity Documentation is pretty good, so I suggest going to their website and looking up anything that seems unfamiliar in the index: [Solidity Language Documentation](https://docs.soliditylang.org/en/v0.8.4/index.html)
<div markdown="1" align=center>

![](/assets/hardhatlogo.png) 
</div>
<h2 id="heading--hardhatdeploy">Deploying a smart contract via Hardhat</h2>

Now that we have our smart contract, lets deploy it to the Ethereum testnet. Before we do that we need to hook up our Ethereum Node API (Alchemy) to our project, and of course we will need to get a Ethereum address (using Metamask). In the root project directory type

> npm install dotenv --save

This will retrieve the configuration module that will interface with Hardhat. Create a new .env file using

> touch .env

add to it two lines, shown below, replacing the strings with the appropriate key from the Alchemy dashboard or Metamask. (If you are unfamiliar with how to use Metamask, see [this article](https://metamask.zendesk.com/hc/en-us/articles/360015289632-How-to-Export-an-Account-Private-Key) on how to get your key).

>API_URL = "https://eth-ropsten.alchemyapi.io/v2/your-api-key"
>PRIVATE_KEY = "your-metamask-private-key"

Now we install the Ethers.js library, which will allow us a easy way to interact with our Ethereum supernode

```
npm install --save-dev @nomiclabs/hardhat-ethers ethers
```
Update our `hardhat.config.js` file to use the dependencies and set our network to be the Ethereum testnet:

```
/**
* @type import('hardhat/config').HardhatUserConfig
*/

require('dotenv').config();
require("@nomiclabs/hardhat-ethers");

const { API_URL, PRIVATE_KEY } = process.env;

module.exports = {
   solidity: "0.7.3",
   defaultNetwork: "ropsten",
   networks: {
      hardhat: {},
      ropsten: {
         url: API_URL,
         accounts: [`0x${PRIVATE_KEY}`]
      }
   },
}
```
We're ready to **compile** now, so go ahead and type the following command in the root of the project directory:

> npx hardhat compile

With luck, you'll see `Compilation finished successfully` after a short moment or two. Great! Now that we've written the smart contract, we need to write a script to do the deployment. In the scripts/ directory make a new file `deploy.js` with these contents:

```
async function main() {
   const HelloWorld = await ethers.getContractFactory("HelloWorld");
   
   // Start deployment, returning a promise that resolves to a contract object
   const hello_world = await HelloWorld.deploy("Hello World!");   
   console.log("Contract deployed to address:", hello_world.address);
}

main()
  .then(() => process.exit(0))
  .catch(error => {
    console.error(error);
    process.exit(1);
  });
```
Briefly, `async function main()` declares a asynchronous function main() which will be the entry point of our program. `const HelloWorld = await ethers.getContractFactory("HelloWorld");` creates a wrapper around the compiled code and returns a promise upon completion (if you aren't familar with promises or the await syntax, [promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises) and [await syntax](https://developers.google.com/web/fundamentals/primers/async-functions) have more information). `const hello_world = await HelloWorld.deploy("Hello World!");` then deploys the contract onto the blockchain, calls the contract contstructor with the string "Hello World!" and then returns an Ethereum address where the contract is located. The rest of the code is boilerplate that calls the function main() and either exits with a successful error code 0 or throws an error and exits with a 1.

We're in the home stretch now. Go ahead and run

>npx hardhat run scripts/deploy.js --network ropsten

You should see something like `Contract deployed to address: 0x2827fD30d8230B1401d0451282e9959FFAa1ADaa` **which means our deployment was successful.**

This can be confirmed by going to the [Ethereum blockchain explorer](https://ropsten.etherscan.io/) and pasting our contract address, to ensure the transaction has been mined.

<h2 id="heading--readwrite">Reading and writing with our smart contract</h2>

Now that we've launched our contract into the chain, we want to try reading and writing to it. Create a new file in the scripts/ directory called `contract-interact.js` with these contents
> 
> require('dotenv').config();
> const API_URL = process.env.API_URL;
> 
> const ethers = require('ethers')
> const network = "ropsten";
> 
> const provider = ethers.getDefaultProvider(network, {
>     alchemy: API_URL,
> });
> 
> const contract = require("../artifacts/contracts/Hello.sol/HelloWorld.json");
> 
> //console.log(JSON.stringify(contract.abi))
> 
> const contractAddress = "0x7d35839ccFfEeD4155dF2A0C6d5c673239568c03";
> 
> const helloContract = new ethers.Contract(contractAddress, contract.abi, provider);
> 
> 
> async function main() {
>  const message = await helloContract.message()
>  console.log("The message " + message);
> }
> main();

Some of the code is self explanatory. We load the 'dotenv' package in the first line since we need to use Alchemy and the API Key we defined earlier. Then we "require" the ethers library to interact with the smart contract. `const contract = require("???/artifacts/contracts/Hello.sol/HelloWorld.json");` allows us to load the contract ABI for use within ethers. What this means is the functions we defined, after compilation, are lost, but the JSON file allows us to still utilize the contract from a high level. Think of it like symbols defined in assembly code, the JSON contains those symbols which makes working with the contract straightforward. The next two lines define the contract address and load the contract using `ethers.Contract`. Finally we declare a asynchronous function to call `helloContract.message()` which returns a promise. If everything went as expected you should see output `The message Hello World!`, which confirms we are interacting with the contract on the blockchain.

Above the function main() copy and paste the following

```
const PUBLIC_KEY = "0xAee835F87074b7d7b26Fe51a5ab60e8bAF468616"
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const wallet = (new ethers.Wallet(PRIVATE_KEY)).connect(provider) //Connect our wallet to Alchemy

async function updateMessage(newMsg) {
 const nonce = await provider.getTransactionCount(PUBLIC_KEY, 'latest') //Needed to prevent replay attacks
 const gasEstimate = await helloContract.estimateGas.update(newMsg) //Estimate gas for transaction
 const gasPriceEstimate = await provider.getGasPrice() //Estimate gas price for transaction

 //craft transaction
 const tx = {
   'from': PUBLIC_KEY,
   'to': contractAddress,
   'nonce': nonce,
   'gasLimit': gasEstimate,
   'value': ethers.BigNumber.from(0),
   'gasPrice': gasPriceEstimate, 
   'data': helloContract.interface.encodeFunctionData("update", [newMsg] )
 }

 try {
   const resp = await wallet.sendTransaction(tx); // Sign and send transaction
   const receipt = await resp.wait() //Wait for transaction to confirm
   console.log(receipt.transactionHash)
  }
  catch (err) {
    console.log("Error")
    console.log(err)
  }
}
```
and then add `await updateMessage("Hello NEW world")` to the end of main(). Re-run the script and you should have seen your transaction on the blockchain, congrats, you have written to your contract! If you call the program again, you will see your message has changed to `The message Hello NEW world!`

I did go fast through that last part, but if you have questions drop a line below. 

[details="Full program"]
```
require('dotenv').config();
const API_URL = process.env.API_URL;
const ethers = require('ethers')
const network = "ropsten";
const provider = ethers.getDefaultProvider(network, {
    alchemy: API_URL,
});

const contract = require("../artifacts/contracts/Hello.sol/HelloWorld.json");
//console.log(JSON.stringify(contract.abi))
const contractAddress = "0x7d35839ccFfEeD4155dF2A0C6d5c673239568c03";
const helloContract = new ethers.Contract(contractAddress, contract.abi, provider);

const PUBLIC_KEY = "0xAee835F87074b7d7b26Fe51a5ab60e8bAF468616"
const PRIVATE_KEY = process.env.PRIVATE_KEY;
const wallet = (new ethers.Wallet(PRIVATE_KEY)).connect(provider) //Connect our wallet to Alchemy

async function updateMessage(newMsg) {
 const nonce = await provider.getTransactionCount(PUBLIC_KEY, 'latest') //Needed to prevent replay attacks
 const gasEstimate = await helloContract.estimateGas.update(newMsg) //Estimate gas for transaction
 const gasPriceEstimate = await provider.getGasPrice() //Estimate gas price for transaction

 //craft transaction
 const tx = {
   'from': PUBLIC_KEY,
   'to': contractAddress,
   'nonce': nonce,
   'gasLimit': gasEstimate,
   'value': ethers.BigNumber.from(0),
   'gasPrice': gasPriceEstimate, 
   'data': helloContract.interface.encodeFunctionData("update", [newMsg] )
 }

 try {
   const resp = await wallet.sendTransaction(tx); // Sign and send transaction
   const receipt = await resp.wait() //Wait for transaction to confirm
   console.log(receipt.transactionHash)
  }
  catch (err) {
    console.log("Error")
    console.log(err)
  }
}

async function main() {
 const message = await helloContract.message()
 console.log("The message " + message);
 await updateMessage("Hello NEW world!");
}
main();
```
[/details]
