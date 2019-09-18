
# Documents

# What, Why and How? about Leo

Leo project is a layer 2 solution on top of Elastos, the existing layer one blockchain. I provide a trusted subset of existing Elastos Carrier P2P network. Only hardware (TPM) attested node can join the network and provide trusted computing service to clients. Leo, the token, is used in this community like "gas" to Ethereum. Leo node owners provide trusted computation services in reward of Leo and a credit score called "credit". The "credit" acts like the "stake" in PoS based consensus. However, credit is not currency. It cannot be trade or purchased. The only way to gain credit is to work honestly for long period of time.

Please read the presentation slides by following this [link](./presentations/Blockchain_and_TrustedComputing_Apr2019.md): The presentation in The World Bank Group Apr 2019.

The reasons we need such a trusted computing network lays on the following facts:
* Blockchain can bring us the trustless decentralized platform. However, blockchain cannot handle real computation. Anything need some kind of "real' computation, you just cannot run it on blockchain. You have to consider run off-the-chain.
* As long as the computation happpens off the chain, the consensus of blockchain can no longer protect the computation. The computing environment is no longer trustable. 
* Is the computing tasks include sensitive privacy data, or copyrighted information. The only thing you can do is to trust the computing machine is secured, the owner is honest, whereas in many cases just not trustable.
* If we have a technical solution to guarantee the execution environment is exactly what the execution node advertised. The whole computing process is under seamless remote attestation by unpreditable inspector (or monitors), we do not have to trust the node and node's owner. We just trust the consensus between all nodes. this is the spirit of the blockchain consensus: "I do not have to trust anyone, but the consensus". This consensus is the core of Leo. And this is why we have the Leo project started in 2019.

# Run a demo
Currently we have developed a simulator to explain the concept. This is just a simulator, not the real product. The goal of the demo is trying to use some real life example to show how the consensus were made and why it is "technically" trustable while the nodes who runs your data are trustless.


## Do not want get hands dirty? watch the video

TODO:

## Ready to get hands dirty? Let's run the code on your own



### Environment

At this moment (Sep 2019), the demo code is written in Node.js. At the time you read this document, we probably have migrate some of the module from Node.js to Rust. ([here is the reason why we start with Node.js then Rust](./demo-doc/reasons_to_started_with_nodejs.md)). So make sure when you check out the code fromn github, always go to the branch demo201909 for all three repos. We will have a release tag someday later.

What you will need:
* A Mac or Linux machine, or A set of Raspberry Pi (at least 5 units in case you do not have a PC/Mac)
* We have not tested in Windows, but it should work as well.
* Install Node.js and Docker in your machine / or Raspberry Pi.
* In this demo, you do not have to install any blockchain client yet, since we actually did not use any blockchain. Everything is simulator.

### Clone the source code

We have three repos on github.
They are:

#### Blockchain (Layer One) Simulator 
Repo: [https://github.com/elastos/Leo.Simulator.Layer1](https://github.com/elastos/Leo.Simulator.Layer1)

This is the simulator of a blockchain. For example Elastos eth side chain. In this demo, all gas and credit settlement are done in the smart contract in the layer one blockchain. In the real world, this will be the samrt contract of blockchain. 

#### Node Simulator
Repo: [https://github.com/elastos/Leo.Simulator.Node](https://github.com/elastos/Leo.Simulator.Node)

This is the simulator of trusted computing node. It can be running on a Raspberry Pi with TPM installed. In the simulator, the code can be run in any Node.js VM. So you should run at least 10 instances in 10 terminals to simulate 10 nodes. If you machine is powerful enough, I would suggest you to run more than 20 nodes simutanously. 

#### Web UI
Repo: [https://github.com/elastos/Leo.Simulator.Web](https://github.com/elastos/Leo.Simulator.Web)
This is not a web server. It is just a index.html file with some js. It doesn't need any web server to host. You just open the local index.html in any web browser. 

Please `git clone`  each repo, and make sure you `git checkout demo201909` so that you choose the correct branch. 

Go to each repo and read the README.md file on how to setup and run the code. 
LayerOne [readme](https://github.com/elastos/Leo.Simulator.Layer1/README.md)
Node [readme](https://github.com/elastos/Leo.Simulator.Node/README.md)
WebUI [readme](https://github.com/elastos/Leo.Simulator.Web/README.md)

Before you can run the demo, make sure you run this docker image if you have some difficulties to access the public internet (For example, you can in China). This docker image will run a local p2p swarm server so that all your nodes can communicate locally without access global p2p swarm servers /dns4/ws-star.discovery.libp2p.io/tcp/443/wss/p2p-websocket-star
```bash
docker pull libp2p/websocket-star-rendezvous:release
docker run -d -p 9090:9090 --name rendezvous libp2p/websocket-star-rendezvous:release
```
BTW, To disable prometheus metrics run the server with -e DISABLE_METRICS=1

# Demo use case

