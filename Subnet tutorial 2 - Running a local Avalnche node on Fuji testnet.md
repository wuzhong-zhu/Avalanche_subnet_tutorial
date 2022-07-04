# Avalanche subnet tutorial series: Running a local Avalanche node on Fuji testnet

![img0](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img0.jpeg?raw=true)

This is the second article in Avalanche subnet tutorial series. Check out the other articles here:

1. What is a subnet.
2. Running a local Avalanche node on Fuji testnet. <– You are here.
3. Creating a subnet, then create a blockchain on it.
4. Deploying a smart contract.
5. Indexing subnet with The Graph.
   
In this article, we’ll guide you for educational purposes in setting up a personal Avalanche node on a local machine and making it a validator. Also, talk to us if want a subnet set up with Chainstack.

The entire process is split into three parts:

1. Setting up [Git with GitHub](https://github.com/) and [Go](https://go.dev/).
2. Running a local Avalanche node.
3. Getting AVAX and [stake](https://docs.avax.network/learn/platform-overview/staking/) the node to make a validator node.

![img1](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img1.jpeg?raw=true)

Warning: this tutorial is crafted for macOS. Developers should adjust accordingly depending on their system. 

---
## Setting up Go
The first step is setting up Go. Go is exceedingly popular for blockchain VMs, and it is also the default language for AvalancheGo. Blockchain developers using a recent version of macOS should already have Go on their machine—run go version in your terminal to confirm. If Go is not installed, [download and install the package](https://go.dev/doc/install).

In rare instances, developers may have issues running the AvalancheGo build script. That is because of a missing environment variable. If that happens, try adding $GOPATH/bin to your $PATH by running the following command:

`export PATH="$PATH:$(go env GOPATH)/bin" `

---

## Setting up Git

Git is another must-have tool for developers. Run the following command to check if it is already installed.

`git version`

If not, [install it](https://github.com/git-guides/install-git).



---
## Setting up node 
Download the [AvalancheGo](https://github.com/ava-labs/avalanchego) source code:

`git clone git@github.com:ava-labs/avalanchego.git`

If it does not work and shows the following error message:

`git@github.com: Permission denied (publickey). `


This is because the SSH key is missing from the SSH agent. Follow [this article](https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent) to resolve this issue. When Git is ready to go, run the command again to download the source code.

After the download is finished, navigate to the avalanche go folder:

`cd avalanchego`

To build the source code, run:

`./scripts/build.sh`

When it is done, run the following command to start the node for Fuji testnet.

`./build/avalanchego --network-id=fuji`

In the terminal, you will see:

![img2](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img2.png?raw=true)

This means the node is up and running. If it is the first time it runs, it needs to synchronize data with the Avalanche primary network. The bootstrapping process takes approximately 50–100 hours and requires 100 GB of space. The node is unusable before it is done. The endpoints are reachable, but most RPC calls respond with error messages.

The bootstrapping status can be checked by sending the following cURL request:

` curl -X POST --data '{ 
   "jsonrpc": "2.0", 
   "method": "info.isBootstrapped", 
   "params":{ 
    "chain":"X" 
   }, 
   "id": 1 
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info `

Now it is wait time.

![img3](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img3.png?raw=true)



Or is it more like:

![img4](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img4.png?raw=true)


The good news is that shutting down the node does not terminate the process. The next time the node is turned on, it’ll start from where it was left.
When the process is complete, you will see:
![img5](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img5.png?raw=true)

Hooray, your node is up and running.
![img6](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img6.png?raw=true)

It is not a validator yet, but it is part of the Avalanche primary network already and developers can now access all three chains through this endpoint. 



---
## Making a validator node

The official documentation explains how to set a validator node on the mainnet and the steps are 90% the same for the Fuji testnet.

First, get your node ID by running the following cURL request:

` curl -X POST --data '{
"jsonrpc":"2.0",
"id" :1,
"method" :"info.getNodeID"
}' -H 'content-type:application/json;' 127.0.0.1:9650/ext/info `

The response is:
![img7](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img7.png?raw=true)

Now visit Avalanche’s official [wallet](https://wallet.avax.network/) website and create a new wallet for your validator. Remember to change the network to Fuji.

![img8](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img8.png?raw=true)


Since it is a new wallet, it is empty. The validator node needs some AVAX to start validating transactions. To get testnet AVAX, copy its X-chain address.
![img9](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img9.png?raw=true)

Go to the [official faucet page](https://faucet.avax-test.network/) to request for testnet faucet. You should receive 2 AVAX on the X-chain.
![img10](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img10.png?raw=true)

Go to the cross-chain tab, and transfer 1 AVAX to P-chain.

![img11](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img11.png?raw=true)
Go to the Earn tab and click Add validator.
![img12](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img12.png?raw=true)

Fill in the node ID and AVAX and click on Confirm.
![img13](https://github.com/wuzhong-zhu/Avalanche_subnet_tutorial/blob/main/resources/tutorial%202/img13.png?raw=true)

Congratulations, you have a validator node now!



---

## Conclusion

In this article, we have shown you how to set up and run an Avalanche node locally and make it a validator of the Fuji testnet. In the next article, we will go into creating our own subnet, so make sure you will not miss them.  

Happy coding.