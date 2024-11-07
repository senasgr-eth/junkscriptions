thxs to https://github.com/blackluv https://github.com/hieujson and 
https://github.com/senasgr-eth !



---

# Junkscriptions

**Junkscriptions** is a minter and protocol for creating inscriptions on the Junkcoin blockchain, allowing for data inscriptions directly onto the blockchain.

## Setup

### Prerequisites

Ensure that you have [Node.js](https://nodejs.org/) and [npm](https://www.npmjs.com/) installed.

### Step 1: Install Dependencies

Run the following command to install required dependencies:

```bash
npm install
```

### Step 2: Configure Environment Variables

Create a `.env` file with your Junkcoin node information:

```plaintext
NODE_RPC_URL=http://<ip>:9771
NODE_RPC_USER=13456
NODE_RPC_PASS=13456
TESTNET=false
FEE_PER_KB=100000000
```

### Step 3: Configure junkcoin.conf

Set up your Junkcoin node configuration file (`junkcoin.conf`) with the following settings:

```plaintext
rpcuser=13456
rpcpassword=13456
rpcport=9771
rpcallowip=127.0.0.1
dns=1
irc=1
listen=1
dnsseed=1
daemon=1
server=1
debug=1

# Nodes to connect with
addnode=159.89.85.93:9771
addnode=157.173.198.7:9771
addnode=72.5.43.135:9771
```

Save `junkcoin.conf` in your Junkcoin data directory (usually located at `~/.junkcoin` on Linux or `%APPDATA%\Junkcoin` on Windows).

---

## Funding

### Generate a New Wallet

Generate a new `.wallet.json` file by running:

```bash
node . wallet new
```

*This command generates a new wallet file. Send Junkcoin to the displayed address. After sending funds, sync your wallet as described below.*

### Sync Your Wallet

Once your wallet has received funds, synchronize it with the network:

```bash
node . wallet sync
```

### Split UTXOs for Bulk Minting

If you plan to mint multiple inscriptions, you can split your UTXOs to have smaller, manageable amounts:

```bash
node . wallet split <count>
```

### Send Remaining Funds

After minting, you can send the remaining funds back to another address:

```bash
node . wallet send <address> <optional amount>
```

---

## Minting

### Mint from a File

You can mint inscriptions directly from a file using:

```bash
node . mint <address> <path>
```

### Mint Repeatedly

To mint multiple copies of the same file:

```bash
node . mint <address> <path> <repeat>
```

### Examples

Mint an inscription from a file:

```bash
node . mint 7VWyhBsDr81bCT8ofMyCMePP8nwAHjiZLK dog.jpeg
```

Mint multiple inscriptions from a JSON file:

```bash
node . mint 7VWyhBsDr81bCT8ofMyCMePP8nwAHjiZLK mint.json 100
```

---

## Starting the Server

To start the Junkscriptions server, use:

```bash
node . server
```

Then, open your browser and navigate to the following URL to view transaction details:

```plaintext
http://localhost:3000/tx/<transaction_id>
```

Replace `<transaction_id>` with the actual transaction ID.

---

## Protocol

The Junkscriptions protocol allows any size data to be inscribed onto subwoofers within Junkcoin.

### Inscription Structure

An inscription is defined as a series of push datas in the following format:

```plaintext
"ord"
OP_1
"text/plain; charset=utf8"
OP_0
"Hello Junkcoin!"
```

### Chunked Content

For larger inscriptions, content can be spread across multiple parts:

```plaintext
"ord"
OP_2
"text/plain; charset=utf8"
OP_1
"Hello "
OP_0
"Junkcoin World!"
```

This example would be concatenated as `"Hello Junkcoin World!"`, allowing up to approximately 1500 bytes per transaction.

### P2SH Encoding

Junkscriptions use P2SH to encode inscriptions. Any P2SH script may be used, as long as the redeem script starts with inscription push data.

### Chained Inscriptions

Inscriptions are allowed to chain across transactions:

**Transaction 1:**

```plaintext
"ord"
OP_2
"text/plain; charset=utf8"
OP_1
"Hello "
```

**Transaction 2:**

```plaintext
OP_0
"Junkcoin!"
```

Each inscription part after the first should start with a countdown separator. Indexers can use this to track remaining data.

---

## FAQ

### I'm getting ECONNREFUSED errors when minting

This error indicates an issue with your node connection. Ensure your `junkcoin.conf` file is set up as follows:

```plaintext
rpcuser=13456
rpcpassword=13456
rpcport=9771
server=1
```

Make sure `rpcallowip` is set to allow local connections, and check that the `rpcport` matches the port specified in `.env`.

### I'm getting "insufficient priority" errors when minting

This error typically means the miner fee is too low. Increase the fee by updating your `.env` file:

```plaintext
FEE_PER_KB=300000000
```

The default is `100000000`, but you may need to increase it during times of high demand.

---

## License

This project is licensed under the MIT License. See the `LICENSE` file for details.

---

