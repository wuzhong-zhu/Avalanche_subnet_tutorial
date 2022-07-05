# Avalanche subnet tutorial series(4): Deploying a smart contract

![img0](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img0.jpeg?raw=true)

This is the second article in Avalanche subnet tutorial series. Check out the other articles here:

1. What is a subnet.
2. Running a local Avalanche node on Fuji testnet.
3. Creating a subnet, then create a blockchain on it.
4. Deploying a smart contract.  <– You are here.
5. Indexing subnet with The Graph.
   
This project has come a long way. At this point, a **local node has been set up**; then it is **staked** and is a validator node for the Fuji testnet; then a **subnet was created**, and the node was assigned to **validate the subnet**; then a **brand new blockchain was established** on it.

Just a quick recap:

- The name of the blockchain is chainstacksubnet;
- Its ID is *2t27BUokp5Pj8zNQhSNVgqYHChSqrd4k8ewSBrEMXoixxjsXQj*. (The ID is unique for every blockchain and subnet).
- It is a Ethereum Virtual Machine (EVM) blockchain. Based on the genesis block information, 333.33 blockchain token was pre-stored in the address **8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC**.

So, the first objective of this tutorial is to transfer some of the tokens out.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img1.jpeg?raw=true)

---
## Connecting to MetaMask

Like every other EVM blockchains, subnet EVM too supports the (arguably) most widely used tool-MetaMask. This section guides the reader to configure [MetaMask](https://metamask.io/) to access a subnet.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img2.png?raw=true)

Firstly, open MetaMask, click **Add Network**:


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img3.png?raw=true)

Fill in the following information:

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img4.png?raw=true)

- **Network Name**: *any name is ok
- **New RPC URL**: *http://127.0.0.1:9650/ext/bc/*fill in your blockchain id/rpc* Example: 
`http://127.0.0.1:9650/ext/bc/
2t27BUokp5Pj8zNQhSNVgqYHChSqrd4k8ewSBrEMXoixxjsXQj/rpc`

- **Chain ID**: 13213
- **Currency Symbol**: *any symbol is ok

Now MetaMask should successfully link to the blockchain. It now can be used to access the initial address. The initial address is defined during creation of genesis block.

Its public address is **0x8db97C7cEcE249c2b98bDC0226Cc4C2A57BF52FC**.

Its private key is *56289e99c94b6912bfc12adc093c9b51124f0dc54ac7a766b2bc5ccf558d8027*.

This private key is publicly shared, it is also given in the [official tutorial](https://docs.avax.network/build/tutorials/platform/subnets/create-evm-blockchain). **Do not use this account on mainnet or testnet.**

Now import the private key into MetaMask.

Click the fox head to open MetaMask. Click on the top right icon, click **Import Account**:

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img5.webp?raw=true)

Key in the private key and click Import:

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img6.webp?raw=true)

The account should be added into MetaMask. A fresh account should contain 333.3333 tokens.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img7.png?raw=true)

Now funds can be sent to another account by clicking on **Send** button.

Input an address or select **Transfer between my accounts**. Complete the steps to send 30 tokens to another account. The genesis specified the minimum base fee as 13000000000 which is equivalent to 0.0000000144 native token.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img8.png?raw=true)

Once the transaction is finalised, it is shown in the **Activity** section. The blockchain is successfully deployed and compatible with MetaMask.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img9.png?raw=true)

---

## Setting up Git

Git is another must-have tool for developers. Run the following command to check if it is already installed.

`git version`

If not, [install it](https://github.com/git-guides/install-git).



---
## Sending smart contract with Remix

Next step is deploying a smart contract on the blockchain. [Remix](https://remix.ethereum.org/) is used to do that.

Remix is a popular IDE for creating/deploying smart contracts on Ethereum based blockchains. It can be used with Avalanche subnet EVM blockchain too.

First, make sure the previous section is finished successfully and MetaMask is connected.

Log in to [Remix](https://remix.ethereum.org/). In the left-most pane, click on file explorer logo.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img10.png?raw=true)

Right-click on contracts folder, create a new file.

Name the file *Gravatar.sol* (any name is ok).

Go to [Etherscan.io](https://etherscan.io/), search for *0x2E645469f354BB4F5c8a05B3b30A929361cf77eC*. This is the contract for Gravatar Registry, it will be moved to our subnet.

Scroll down to the lower half of the page, click the **Contract** tab, copy its source code.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img11.png?raw=true)

Go back to Remix, paste it into *Gravatar.sol* and go to the solidity compiler in the left pane.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img12.png?raw=true)

Compile *Gravatar.sol*. When it is finished, you will see **GravatarRegistry** in the contract dropdown menu. It means the contract is successfully compiled. Next step is to deploy it to the subnet blockchain.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img13.png?raw=true)

Click **Deploy** tab in the left pane.

Set environment to **Injected Web3** to use MetaMask.

Select an account with sufficient funds.

Select the contract GravatarRegistry.

Click **Deploy**.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img14.png?raw=true)

Click confirm on MetaMask pop-up.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img15.png?raw=true)

It takes a while to finish the transaction. After the smart contract is successfully deployed, it can be tested in the Remix IDE.

In the **Deploy** tab, a new section shows up now for **Deployed Contracts**. The methods from the *GravatarRegistry* contract are displayed there. It means the contract is successfully deployed. Feel free to play around with all the methods.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img16.webp?raw=true)

Click on **createGravatar**, enter a *displayName* and *imageURL*, click **transact**.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img17.png?raw=true)

Finish this step in MetaMask by clicking on the Confirm button.


![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img18.png?raw=true)

The transaction shows up in the activity section on MetaMask too:

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img19.webp?raw=true)

Use Remix to test the *getGravatar* function with the owner’s address, it returns the image URL.

![img](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%204/img20.png?raw=true)

Now the smart contract is successfully deployed on the subnet.



---

## Conclusion
This is the end of this article. In this article we deployed a smart contract on our blockchain using Remix and MetaMask. Hoping it is useful.

Happy coding.
