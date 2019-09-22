
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
Here the story comes:
Alice has a sensitive private picture. She want someone to help her to turn the picture from color to black and white. She doesn't have the software or doesn't know how to use the image processing software. Well there are some online services provide can do that, however Alice has to upload the image to some kind of cloud which Alice doesn't trust. She is afraid of the sensitive private picture disclosed since she has no control as long as the picture were uploaded anywhere on the internet. 

Bob is a software developer. He developed a algorithm to turn a color image into black & white. Typically he can upload his code to a web server and provide kind of service to make some money. He did not go this way because he realize many clients do not trust him or the web server and not willing to upload their private pictures. Some clients ask Bob if he can send them the code so that they can run the algorithm in their own computer. Bob has big concern on this because he is afraid of disclose the code so that no one else would like to pay him any more. Bob is looking for a pure decentralized trusted computing platform so that he or anyone else , even the owner of the node running the process, cannot steal the private images. This is how Leo can do for Bob and Alice. 
## Step 1 Bob: Offer decentralized image processing service on Leo
First, Bob need to encrypt his code and upload to the Leo platform as a Lambda Transaction. In this transaction, Bob provide the price model "Pay per use" at the price of $Leo 3. Because Bob uploaded the his code in encrypted format, no one can decrypt unless Bob give him the secure key. In this case, Bob is safe. 
Bob also provide a hash of the docker image which can run his code. Bob's code is written in Python, so he just need to find a standard Python docker image which can be find in dockerhub anywhere. 

Next, Bob just need to do some advertisement so that the potential customer will know the hash of his paid service. 

## Step 2 Alice: Offer an image processing task to Leo
Alice found Bob's offer. Alice doesn't trust Bob, but Alice trust the Leo platform. Alice use her client to encrypt her private picture then upload to Leo as a new transaction. In this transaction she claim to use Bob's service (mentioned his service hash), and the requirement of pravacy: such as 1) The executor or monitor has to have a credit score 100 or above. 2)For executor's node, no network connection during process, clean up all cache, tmp file and trace after processing. make sure leave no trace. And the last not least, her offer: she is willing to pay $10 for this task.

## Step 3 VRF Select execution group members
Once Alice's transaction broacasted to the Leo network, any nodes who meet the critial can run a VRF function to determin if he is lucky enough to bid this task. Based on the default configuation ( or Alice can have a spcial config) there are 10 nodes passed the VRF and become "members of execution group". Because the randomness of VRF, the number sometimes could be a little higher or lower than 10, but it doens't matter. 

## Step 4 Members of execution group run consensus to vote the leader - AKA Executor
The members of execution group will connect to each other using P2P communication channel, exchange their "random value with proof of VRF" and "voting power" - so called "J" value. The highest J value ( if J equals, the lower random value) will be the leader of the group, or so called "executor". Other members as long as passed the VRF and get verified would become monitors. Executor is the one who actually run the image process, as well as monitors are those node who do not run the process, but doing remote attestation before, during and after execution's process. All of them will get rewarded after the task is done unless they lie. If they did lie, there would be a penalty instead.

## Step 5 Executor and monitors provide proof to Alice and Bob's nodes, asking for secure keys
Alice and Bob will receive executor's request for secure key. Before they can send the secure key back to executor, they will need to verify the executor's proof of VRF and other monitor's confirmation, so that they are confident that this executor is actually the real executor and its execution environment meet the requriement. The secure key itself will be RSA encrypted using executor's public key so that only executor can unlock it.

## Step 6 Executor receive secure key and decrypt the code and data in the trusted environment and execute
Executor receive and decrypt secure key. Use this secure key he can unlock both code and image data only inside the trusted execution environment (docker). this is guaranteed by and under the remote attestation from monitors.

After the execution, the result (the black & white image) is sent to Alice with the encrypted key RSA by Alice's public key. So only Alice can unlock the image. 

Once done, the executor will clean up the docker image, all tmp files or memory trace. Monitors will confirm this by doing remote attestation using TPM. All of the monitors will sign the confirmation and send to layer one blockchain. Executor send the request-for-payment to layer one blockchain too.

## Step 7 Settlement
Layey one blockchain receive all the confirmations from monitor and request from executor. Smart contract is triggered to calculate the reward and penalty. The current distribution is this: Alice paid $Leo 10. $1 burn as gas fee. Bob get $3 (as he offerred his algorithm at $5 per use). The rest $6 will be distributed to exeuction group. The exeuction get 50%, which is $3. Other 9 monitors share another 50%, which is $0.33 per monitor.

The new balance will be included in next block. 

# 