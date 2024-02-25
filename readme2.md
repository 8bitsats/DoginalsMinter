A minter and protocol for inscriptions on Dogecoin. 

Forked from Apezord with Love by 8 bit.

Prerequisites:

Download and install NODE.js

Head to https://nodejs.org/en/download and download Nodejs for Mac or PC

Launch your own RPC
In order to inscribe, you will need to have access to a Dodgecoin RPC. 

Go to https://go.getblock.io/ and sign up for an account.

Set up a shared Dogecoin Node, to mainnet, with json RPC and click get.

Copy that address and that is your brand new NODE RPC to use for this minter.

It is very important to save that for future use, for your .env file.

Alright let's get to inscribing shall we?

1. Install Dependencies 

Open Terminal
Copy This Code and paste it into terminal

npm install

2. Setup

Create a ".env" file with your node information.
I have provided one already within this file, for you to work off of. 
The example .env file looks like this:

NODE_RPC_URL=https://go.getblock.io/ (Place your node information)
TESTNET=false
FEE_PER_KB=1000000000 <--------- This is very important!!!!

That is the fee rate that you choose to pay to miners per inscription.

Before inscribing, go to https://sochain.com/DOGE and check the urgent fee rate to determine.

3. Funding
Steps for generating a new .wallet.json file, sending DOGE to the address, syncing the wallet, splitting UTXOs, and sending funds back.

Generate a new `.wallet.json` file:

Open up your terminal and copy this code below and paste it and execute:

node . wallet new

Then send DOGE to the address displayed. Once sent, sync your wallet:

You sync your wallet up by using this code in your terminal:

node . wallet sync

If you are minting a lot, you can split up your UTXOs:

You do this by using this code in your terminal and executing the code.

node . wallet split <count>

When you are done minting, send the funds back:

By entering this code in the terminal, and noting the wallet address you want to send funds.

node . wallet send <address> <optional amount>

Enter the address, and the amount, and you remove the "<>"

4. Minting
Instructions on how to mint from a file or directly from data, including examples.

From file:

You want to note the location of the file, you copy the file path location.
Than you note the location and enter it in this code below:

node . mint <address> <path> 

Please note that you must remove the "<>" before executing the code above. 

For example: here is the perfect code: 

node . mint D5ajNi1dsbW3zehjv1wx9J6dseEABrT7zA /Users/8bit/Downloads/artwork.png

From data:

node . mint <address> <content type> <hex data>

node . mint D5ajNi1dsbW3zehjv1wx9J6dseEABrT7zA "text/plain;charset=utf8" 576f6f6621 

5. DRC-20
Guidance on minting and deploying DRC-20 tokens, with examples and a note on wallet usage.

Examples: 

node . drc-20 mint D5ajNi1dsbW3zehjv1wx9J6dseEABrT7zA shit 10
node . drc-20 mint D5ajNi1dsbW3zehjv1wx9J6dseEABrT7zA shit 10
node . drc-20 deploy D5ajNi1dsbW3zehjv1wx9J6dseEABrT7zA dogi 1000

**Note**: Please use a fresh wallet to mint to with nothing else in it until proper wallet for doginals support comes. You can get a paper wallet [here](https://www.fujicoin.org/wallet_generator?currency=Dogecoin).


6. Viewing
Steps to start the server and access inscriptions via the browser.

## Viewing

Start the server

Enter this code in your terminal:

node . server

And open your browser to:

```
http://localhost:3000/tx/f3739a7092f60585af83bfc07c7b93f5da1b366be1b55cfddbe3f9a905a01f15

7. Protocol
Detailed explanation of the Doginals protocol, including inscription definitions, protocol extensions, and chaining across transactions.

## Protocol

The doginals protocol allows any size data to be inscribed onto subwoofers.

An inscription is defined as a series of push datas:

```
"ord"
OP_1
"text/plain; charset=utf8"
OP_0
"Woof!"
```

For doginals, we introduce a couple extensions. First, content may spread across multiple parts:

```
"ord"
OP_2
"text/plain; charset=utf8"
OP_1
"Woof and "
OP_0
"woof woof!"
```

This content here would be concatenated as "Woof and woof woof!". This allows up to ~1500 bytes of data per transaction.

Second, P2SH is used to encode inscriptions.

There are no restrictions on what P2SH scripts may do as long as the redeem scripts start with inscription push datas.

And third, inscriptions are allowed to chain across transactions:

Transaction 1:

```
"ord"
OP_2
"text/plain; charset=utf8"
OP_1
"Woof and "
```

Transaction 2

```
OP_0
"woof woof!"
```

With the restriction that each inscription part after the first must start with a number separator, and number separators must count down to 0.

This allows indexers to know how much data remains.

## FAQ

### I'm getting ECONNREFUSED errors when minting

There's a problem with the node connection. Your `dogecoin.conf` file should look something like:

```
rpcuser=ape
rpcpassword=zord
rpcport=22555
server=1
```

Make sure `port` is not set to the same number as `rpcport`. Also make sure `rpcauth` is not set.

Your `.env file` should look like:

```
NODE_RPC_URL=http://127.0.0.1:22555
NODE_RPC_USER=ape
NODE_RPC_PASS=zord
TESTNET=false
```

### I'm getting "insufficient priority" errors when minting

The miner fee is too low. You can increase it up by putting FEE_PER_KB=300000000 in your .env file or just wait it out. The default is 100000000 but spikes up when demand is high.

8. FAQ
Solutions to common issues such as ECONNREFUSED errors and "insufficient priority" errors.