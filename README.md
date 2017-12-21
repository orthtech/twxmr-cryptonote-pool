twxmr-cryptonote-pool
====================

This is the core of [twxmr mining pool](twxmr.tw) forked from [node-cryptonote-pool](https://github.com/zone117x/node-cryptonote-pool), a high performance Node.js (with native C addons) mining pool for CryptoNote based coins such as Bytecoin, Monero, Electroneum, etc..

For more info check out the original repo.


#### Table of Contents
* [Improvement](#improvement)
* [Usage](#usage)
  * [Requirements](#requirements)
  * [Downloading & Installing](#1-downloading--installing)
  * [Configuration](#2-configuration)
  * [Configure Easyminer](#3-optional-configure-cryptonote-easy-miner-for-your-pool)
  * [Starting the Pool](#4-start-the-pool)
  * [Upgrading](#upgrading)
* [Setting up Testnet](#setting-up-testnet)
* [JSON-RPC Commands from CLI](#json-rpc-commands-from-cli)
* [Monitoring Your Pool](#monitoring-your-pool)
* [Donations](#donations)
* [Credits](#credits)
* [License](#license)

Improvement
===
* `WIP` Multi-pool for both XMR and ETN
* `WIP` Porting to newest Node.js
* `WIP` Optmized for XMR and ETN

Usage
===

#### Requirements
* Coin daemon(s) (twxmr will be optmized for XMR and ETN)
* [Node.js](http://nodejs.org/) v0.10.x (Do not use the one newer than that, working on porting)
* [Redis](http://redis.io/) key-value store v2.6+
* libssl required for the node-multi-hashing module
  * For Ubuntu: `sudo apt install libssl-dev`
* Boost is required for the cryptonote-util module
  * For Ubuntu: `sudo apt install libboost-all-dev`



#### 1) Downloading & Installing


Clone the repository and run `npm update` for all the dependencies to be installed:

```bash
git clone https://github.com/zone117x/node-cryptonote-pool.git pool
cd pool
npm update
```

#### 2) Configuration


*Warning for Cyrptonote coins other than Monero:* this software may or may not work with any given cryptonote coin.
Be wary of altcoins that change the number of minimum coin units because you will have to reconfigure several config
values to account for those changes. Unless you're offering a bounty reward - do not open an issue asking for help
getting a coin other than Monero working with this software.


Copy the `config_example.json` file to `config.json` then overview each options and change any to match your preferred setup.


#### 3) Start the pool

```bash
node init.js
```

The file `config.json` is used by default but a file can be specified using the `-config=file` command argument, for example:

```bash
node init.js -config=config_backup.json
```

This software contains four distinct modules:
* `pool` - Which opens ports for miners to connect and processes shares
* `api` - Used by the website to display network, pool and miners' data
* `unlocker` - Processes block candidates and increases miners' balances when blocks are unlocked
* `payments` - Sends out payments to miners according to their balances stored in redis


By default, running the `init.js` script will start up all four modules. You can optionally have the script start
only start a specific module by using the `-module=name` command argument, for example:

```bash
node init.js -module=api
```

[Example screenshot](http://i.imgur.com/SEgrI3b.png) of running the pool in single module mode with tmux.



#### Upgrading
When updating to the latest code its important to not only `git pull` the latest from this repo, but to also update
the Node.js modules, and any config files that may have been changed.
* Inside your pool directory (where the init.js script is) do `git pull` to get the latest code.
* Remove the dependencies by deleting the `node_modules` directory with `rm -r node_modules`.
* Run `npm update` to force updating/reinstalling of the dependencies.
* Compare your `config.json` to the latest example ones in this repo or the ones in the setup instructions where each config field is explained. You may need to modify or add any new changes.

### Setting up Testnet

No cryptonote based coins have a testnet mode (yet) but you can effectively create a testnet with the following steps:

* Open `/src/p2p/net_node.inl` and remove lines with `ADD_HARDCODED_SEED_NODE` to prevent it from connecting to mainnet (Monero example: http://git.io/0a12_Q)
* Build the coin from source
* You now need to run two instance of the daemon and connect them to each other (without a connection to another instance the daemon will not accept RPC requests)
  * Run first instance with `./coind --p2p-bind-port 28080 --allow-local-ip`
  * Run second instance with `./coind --p2p-bind-port 5011 --rpc-bind-port 5010 --add-peer 0.0.0.0:28080 --allow-local-ip`
* You should now have a local testnet setup. The ports can be changes as long as the second instance is pointed to the first instance, obviously

*Credit to surfer43 for these instructions*



Donations
---------
* XMR: `49JZ6KbG8tF6ZwEL4gfxp2GNoiVx3De4xCk2w25GTFs15VXwkcYggnxUCd4KRkpMtD6JvJfr9ttEaiFqLq5T6Rtp88H9cBC`
* ETN: `etnkFAwchFJPVVBXXbFT2cZn8Dd1FSpgcjo6rXxV23Kd8zYQsf4eY9k8Y1iAkHr6sqHBUTf3xoqbZchyEZZBH7Q12sUpSzJeAG`

Credits
===

* [workfunction](https://github.com/workfunction) - core developer of twxmr.
* [snoopy831002](https://github.com/snoopy831002) - core developer of twxmr.
* [LucasJones](//github.com/LucasJones), [surfer43](//github.com/iamasupernova), [wallet42](http://moneropool.com), [Wolf0](https://bitcointalk.org/index.php?action=profile;u=80740), [Tacotime](https://bitcointalk.org/index.php?action=profile;u=19270) - for there awesome works of node-cryptonote-pool.

License
-------
Released under the GNU General Public License v2

http://www.gnu.org/licenses/gpl-2.0.html
