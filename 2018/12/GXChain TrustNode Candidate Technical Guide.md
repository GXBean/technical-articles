# GXChain TrustNode Candidate Technical Guide

## Overview

GXChain is one of the most influential and promising public blockchains, with the vision of a trusted data Internet of value. It has a consensus model of DPoS, and its Committee and TrustNodes play a vital role in its on-chain governance. The committee constitutes 11 members, who can propose to vote for the modification of global parameters of GXChain. The TrustNodes constitute 21 members, who are in charge of the transaction verification, production, and broadcast of blocks. A GXC holder has a voting power that equals to the amount of GXC he owns. Top 11 voted TrustNodes constitute the GXChain committee.

There are several requirement for GXChain TrustNode Candidates. Other than those informational ones, in the technical aspect, to start a TrustNode Candidate, you need to first setup a TrustNode server, then create a GXChain account with the name you desire, and register it as a TrustNode Candidate.

We’re GXBean, a GXChain TrustNode and Committee member (currently ranked 10), owned by Spark Blockchain. We devote ourselves to GXChain community engagement. Spark Blockchain, rooted from Boston and New York City, expands across Asia and North America as an incubator and developer community. Spark Blockchain is experienced in building and operating block producers. With our experience, we hope to help you get started smoothly.

## Get Started

*P.S. In the context of GXChain, "witness" and "trustnode" means the same thing.*

### Step 1: Create GXChain Account and Register as TrustNode Candidate

*Reference: [Chinese Documentation](https://mp.weixin.qq.com/s/eNQyqY5dyaP299J5qra0Bg)*

To go through all these steps, you will need 10,000 GXC for staking, and a bit more than 51.5 GXC as fees. You can purchase GXC from Binance, Gate.io, BitMart, or any other exchanges with GXC listed.

#### 1. Create a GXChain account

Visit GXChain web wallet at https://wallet.gxb.io/#/create-account to create a GXChain account. You can switch the UI language to English by clicking on the Flag button at the upper right corner. If you encounter this warning message:

> This is a premium name which is not supported by this faucet. Please enter a regular name containing least one dash, a number or no vowels.

Please try to add a dash `-` or a number to your desired account name, or remove all vowels (`a`, `e`. `i`, `o`, `u`).

After setting account name and password, the GXChain web wallet will generate private keys for you in your browser and save it encrypted locally. The wallet will also create your account on the GXChain blockchain for you.

Please make sure to download account backup file and save it properly. If you lose this backup, you will not be able to restore your account.

#### 2. Retrieve your public key and private key

After backup, go to `Account > Permissions > Owner Permissions`, and click on the blue public key in the `ACCOUNT / KEY / ADDRESS` field, you will see a popup `Private key viewer`. Click on the blue `show` button in the `PRIVATE KEY (WIF - WALLET IMPORT FORMAT)` field, you will see your account private key. Copy the public key and private key, you will need them later.

#### 3. Purchase Lifetime membership

In GXChain, you need to be a lifetime member in order to apply for TrustNode candidacy. Lifetime membership costs 50 GXC. To transfer GXC to your account, use your account name as transfer recipient. Go to `Account > Membership`,  click on the `BUY LIFETIME SUBSCRIPTION` button, and confirm your upgrade. When this transaction is confirmed on the blockchain, you will be upgraded to Lifetime Member.

#### 4. Become TrustNode candidate

After upgraded to lifetime member, the `BUY LIFETIME SUBSCRIPTION` button will be replaced by a `BECOME TRUSTNODE CANDIDATE` button. Click on the button, and confirm the transactions. During this step, 10,000 GXC will be staked for your candidacy. When these transactions are confirmed on the blockchain, you will become a TrustNode candidate officially, and you will see a TrustNode ID displayed on this page, like: `TrustNode Candidate 1.6.84`. Please copy the TrustNode ID `1.6.84` and save it for later.

OK, now you have created your GXChain account, and you have your private key, public key and TrustNode ID in your hand. You are ready for launching the server!

### Step 2: Setup Block-producing TrustNode Server

*Reference: [Chinese Documentation](https://docs.gxchain.org/zh/guide/witness.html#_2-%E9%83%A8%E7%BD%B2%E5%B9%B6%E5%90%AF%E5%8A%A8%E5%85%AC%E4%BF%A1%E8%8A%82%E7%82%B9%E7%A8%8B%E5%BA%8F)*

#### 1. Prepare your server.

According to the official doc, you need to prepare a server with at lease 32GB of RAM.

> Server Specification Requirements:  
> Operating System: Ubuntu 14.04 64-bit, 4.4.0-63-generic or higher  
> RAM: > 32GB (The more the better)  
> Hard Drive: > 200GB  
> Network: > 20MB/s  

We recommend you to use bare-metal servers instead of VPS or cloud servers, for better performance, especially in case of blockchain replay.

The TrustNode program will run on macOS. However, the GXChain team, and we, both recommend you to run Ubuntu 14.04 or 16.04, instead of macOS, for the server, also for better performance. 

#### 2. Install dependencies

Install `ntp`.

    sudo apt-get install ntp

Install `libstdc++-7-dev`.

    apt-get update
    apt-get install software-properties-common
    add-apt-repository ppa:ubuntu-toolchain-r/test
    apt-get update
    apt-get install libstdc++-7-dev

#### 3. Download `gxb-core`

`gxb-core` is the implementation of GXChain protocol. You can find its source code at its [GitHub repo](https://github.com/gxchain/gxb-core), or run this script provided by GXChain team to download latest build of `gxb-core` to current directory:

    curl 'https://raw.githubusercontent.com/gxchain/gxb-core/dev_master/script/gxchain_install.sh' | bash

If it takes a long time for this script to download the build (it will download from Aliyun in Hangzhou, China), you may also manually download from [GitHub releases](https://github.com/gxchain/gxb-core/releases) and uncompress.

#### 4. Launch program and wait for initial sync

After downloading `gxb-core` to current directory, you will be able to start the `witness_node` program with the following command:

    ./programs/witness_node/witness_node --data-dir=trusted_node -w '"1.6.10"' \
    --private-key '["GXC73Zyj56MHUxxxxxx", "5JainounrsmxxxxxxPhz2R7Pg8yaZh9Ks"]'

Please remember to replace the parameters provided for options.

    --data-dir      The path for storage of blockchain data. This directory need large available hard drive space.
    -w              The TrustNode ID of your GXChain account. Be careful that you need to include the double quotation " inside of the single quotation '.
    --private-key   Your own GXChain public and private keys.

After the program starts, it may take up to over 30 hours for initial syncing. The program will download blocks from the very beginning. You can check the log file for progress:

    tail trusted_node/logs/witness.log

We recommend you to utilize [`screen(1)`](https://linux.die.net/man/1/screen) when configuring your server.

The log looks like this when initial sync is finished:

    2018-06-28T03:43:03 th_a:invoke handle_block         handle_block ] Got block: #10731531 time: 2018-06-28T03:43:03 latency: 60 ms from: miner11  irreversible: 10731513 (-18)			application.cpp:489

The `#10731531` part is the current block height. To check latest block height, go to [GXChain Block Explorer](https://block.gxb.io/).

When the initial sync is finished, you are all set.

Once you are elected as an active TrustNode, you will see logs like this:

    Generated block #367 with timestamp 2017-08-05T20:46:30 at time 2017-08-05T20:46:30


## Advanced Tricks (Optional)

### Create "Premium" GXChain Accounts

As we mentioned in `Step 1.1.`, you can not create "Premium" accounts in the GXChain web wallet, as they are not free. However, you can use your existing GXChain account to create a "Premium" account with the `cli-wallet`, which is a command line shell program in `gxb-core`. The cost for a "Premium" account is about `1.00002 GXC`.

#### 1. Launch `cli-wallet`

`cd` to the home directory of your `gxb-core` installation, and run this command to start `cli-wallet`:

    ./programs/cli_wallet/cli_wallet -swss://node1.gxb.io -w wallet.json

The first time you starting `cli-wallet`, you will be prompted to set a wallet password. Type in the following command after `new >>>` and press Enter:

    set_password <NewPassword>

`NewPassword` is the password you desire. Please save it properly. The wallet will encrypt your private keys with the password and save them in the `wallet.json` file in current directory.

After the password is set, you need to unlock the wallet. Type in the following command and press Enter:

    unlock <NewPassword>

Now your `cli-wallet` is unlocked.

#### 2. Import your private key

Copy the private key for the existing account from `Step 1.2`, type in the following command and press Enter to import private key:

    import_key <ExistingAccountName> <ExistingAccountPrivateKey> true

Then we will be able to perform actions as your account in `cli-wallet`. 

#### 3. Generate Brain Key

Type in the following command and press Enter to generate a new brain key for your new account:

    suggest_brain_key

The output looks like:

    {
      "brain_priv_key": "ACROAMA DIAZOIC GERS TUP CAUDA SOORKEE CYANIN LOMITA DRYAS STRIAL DESMIC MUCAGO GENESIC YAKMAN CORONER BLURT",
      "wif_priv_key": "5J4YJYdSzJZ4UkJQ8sFpRgGzux3Rg9otUFSjaJSxvcZWQ3JW3md",
      "pub_key": "GXC5SHQX91AEmkBbJZkkJEwec7RS9w3y5GJAsovCDtmXUhj4NiijJ"
    }

The `brain_priv_key` value is the Brain Key you need. In the GXChain context, a Brain Key is like a mnemonic code in other blockchains. With the Brain Key, the wallets could derive private keys. The `wif_priv_key` value is the private key. The `pub_key` value is the public key.

#### 4. Create new "Premium" account

Type in the following command and press Enter to create your new account:

    create_account_with_brain_key "<BrainKey>" <NewAccountName> <ExistingAccountName> <ExistingAccountName> true

After the command is executed successfully, your new account is created.

#### 5. Import new "Premium" account into web wallet

Go to `Settings > Wallet` in the web wallet, and click on the `NEW WALLET` button. Click on `USE A CUSTOM BRAINKEY (ADVANCED)` and paste your Brain Key into the field. Finish the form and click on `CREATE NEW WALLET`, you will be able to use the new account in the web wallet.

### Add URL to your TrustNode Profile

TrustNode Candidates created with web wallet does not have a homepage URL in its profile. Using `cli-wallet`, you will be able to provide a homepage URL when creating TrustNode candidate, or update the profile to add the homepage URL. Type in the following command and press Enter to update the profile:

    update_witness <TrustNodeCandidateAccountName> <HomepageUrl> null GXC true

When the transaction is successfully executed, all GXChain users will be able to see your homepage url when they read the TrustNode candidate list.


## What's Next

### Claiming TrustNode Rewards

*Reference: [Chinese Documentation](https://docs.gxchain.org/zh/guide/witness.html#_3-%E6%9F%A5%E7%9C%8B%E5%85%AC%E4%BF%A1%E8%8A%82%E7%82%B9%E5%87%BA%E5%9D%97%E5%A5%96%E5%8A%B1)*

After you are elected as TrustNode and have produced blocks, you will be able to claim TrustNode rewards. Just go to `Accounts > Vesting Balances > Witness income` in the web wallet.

### Setup API Server

*Reference: [Chinese Documentation](https://docs.gxchain.org/zh/guide/api_server.html)*


## One More Thing

Please vote for us if you are a GXC holder! Vote: ***gxbean***.

If you need help to set up your TrustNode, or have any questions, please feel free to contact us at gxbean@sparkincu.com or @GXBean.

