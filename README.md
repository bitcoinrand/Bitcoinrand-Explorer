Tutorial - Install a block explorer on Ubuntu Server 18.04
Install a block explorer on Ubuntu Server 18.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Install the required dependencies with the following command:

sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev -y

Install the additional dependencies with the following command:

sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip software-properties-common libkrb5-dev mongodb nodejs npm git nano cmake screen -y

Install the repository ppa:bitcoin/bitcoin with the following command:

sudo add-apt-repository ppa:bitcoin/bitcoin

Confirm the installation of the repository by pressing on the enter key. enter

Install Berkeley DB with the following command:

sudo apt-get update && sudo apt-get install libdb4.8-dev libdb4.8++-dev -y

Download the Linux daemon for your wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=84a7e9c17d49ec329a5cf0eda77ad435cbab1df032acc370db&filename=bitcoinrand-daemon-linux.tar.gz" -O bitcoinrand-daemon-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf bitcoinrand-daemon-linux.tar.gz

Download the Linux tools for your wallet with the following command:

wget "https://dl.walletbuilders.com/download?customer=84a7e9c17d49ec329a5cf0eda77ad435cbab1df032acc370db&filename=bitcoinrand-qt-linux.tar.gz" -O bitcoinrand-qt-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf bitcoinrand-qt-linux.tar.gz

Type the following command to install the daemon and tools for your wallet:

sudo mv bitcoinrandd bitcoinrand-cli bitcoinrand-tx /usr/bin/

Type the following command to open your home directory:

cd $HOME

Create the data directory for your coin with the following command:

mkdir $HOME/.bitcoinrand

Open nano.

nano $HOME/.bitcoinrand/bitcoinrand.conf -t

Paste the following text into nano.

rpcuser=rpc_bitcoinrand
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
addnode=node1.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your daemon:

bitcoinrandd

Type the following command to open MongoDB:

mongo

Type the following command to create a MongoDB database named “explorerdb”:

use explorerdb

Type the following command to create a MongoDB user named “iquidus”:

db.createUser( { user: "iquidus", pwd: "414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv", roles: [ "readWrite" ] } )

Type the following command to close MongoDB:

exit

Type the following command to clone iquidus-explorer:

git clone https://github.com/walletbuilders/explorer.git explorer

Type the following command to install iquidus-explorer:

cd explorer && npm install --production

Type the following command to create the file settings.json:

cp ./settings.json.template ./settings.json

Open nano.

nano settings.json -t

Modify the following values in the file settings.json

title - “IQUIDUS” -> “Bitcoinrand”.

address - Change the value “127.0.0.1” with the IPv4 address of your server.

coin - “Darkcoin” -> “Bitcoinrand”.

symbol - “DRK” -> “BZAR”.

password - “3xp!0reR” -> “414uq3EhKDNX76f7DZIMszvHrDMytCnzFevRgtAv”.

use_rpc - “false” -> “true”.

port - “9332” -> “19451”.

user - “darkcoinrpc” -> “rpc_bitcoinrand”.

pass - 123gfjk3R3pCCVjHtbRde2s5kzdf233sa” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

confirmations - “40” -> “20”.

api - “true” -> “false”.

markets - “true” -> “false”.

twitter - “true” -> “false”.

Save the file with the keyboard shortcut ctrl + x.

Type the following command to open a screen session:

screen

Type the following commands to start your block explorer:

cd $HOME/explorer
npm start

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.

Type the following command to open crontab:

crontab -e

Press the Page Down key on your keyboard PgDown.

Paste the following text into crontab.

@reboot bitcoinrandd
*/1 * * * * cd /root/explorer && /usr/bin/nodejs scripts/sync.js index update > /dev/null 2>&1
*/5 * * * * cd /root/explorer && /usr/bin/nodejs scripts/peers.js > /dev/null 2>&1

Save the crontab with the keyboard shortcut ctrl + x

Confirm that you want to save the crontab with the keyboard shortcut y + enter
