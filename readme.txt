Tutorial - Install a mining pool on Ubuntu Server 18.04
Install a mining pool on Ubuntu Server 18.04 with the following tutorial.

Update your Ubuntu server with the following command:

sudo apt-get update && sudo apt-get upgrade -y

Install the required dependencies with the following command:

sudo apt-get install build-essential libtool autotools-dev automake pkg-config libssl-dev libevent-dev bsdmainutils python3 libboost-system-dev libboost-filesystem-dev libboost-chrono-dev libboost-test-dev libboost-thread-dev libboost-all-dev libboost-program-options-dev -y

Install the additional dependencies with the following command:

sudo apt-get install libminiupnpc-dev libzmq3-dev libprotobuf-dev protobuf-compiler unzip software-properties-common redis-server npm git nano cmake screen -y

Install the repository ppa:bitcoin/bitcoin with the following command:

sudo add-apt-repository ppa:bitcoin/bitcoin

Confirm the installation of the repository by pressing on the enter key. enter

Install Berkeley DB with the following command:

sudo apt-get update && sudo apt-get install libdb4.8-dev libdb4.8++-dev -y

Download the Linux daemon for your wallet with the following command:

wget "https://github.com/bitcoinrand/bitcoinrand/releases/download/V1.0.0/bitcoinrand-qt-linux.tar.gz" -O bitcoinrand-daemon-linux.tar.gz

Extract the tar file with the following command:

tar -xzvf bitcoinrand-daemon-linux.tar.gz

Download the Linux tools for your wallet with the following command:

wget "https://https://github.com/bitcoinrand/bitcoinrand/releases/download/V1.0.0/bitcoinrand-daemon-linux.tar.gz" -O bitcoinrand-qt-linux.tar.gz

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

Paste the following into nano.

rpcuser=rpc_bitcoinrand
rpcpassword=dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2
rpcallowip=127.0.0.1
listen=1
server=1
txindex=1
daemon=1
paytxfee=0.0002
deprecatedrpc=accounts
addresstype=legacy
addnode=node1.walletbuilders.com

Save the file with the keyboard shortcut ctrl + x.

Type the following command to start your daemon:

bitcoinrandd

Type the following command to show the receiving address of your daemon:

bitcoinrand-cli getaccountaddress ""

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Type the following commands to install NOMP:

git clone https://github.com/walletbuilders/node-open-mining-portal.git nomp
cd nomp
npm update

Type the following command to create the file settings.json:

cp config_example.json config.json

Open nano.

nano config.json -t

Modify the following values in the file config.json.

host - Change the value “0.0.0.0” with the IPv4 address of your server.

stratumHost - Change the value “cryppit.com” with the IPv4 address of your server.

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your coin:

cd coins
cp bitcoin.json bitcoinrand.json

Open nano.

nano bitcoinrand.json -t

Modify the following values in the file bitcoinrand.json.

name - “Bitcoin” -> “Bitcoinrand”.

symbol - “BTC” -> “BZAR”.

Save the file with the keyboard shortcut ctrl + x.

Type the following commands to create the config file for your pool:

cd ../pool_configs
cp litecoin_example.json bitcoinrand_pool.json

Open nano.

nano bitcoinrand_pool.json -t

Modify the following values in the file bitcoinrand.json.

enabled - “false” -> “true”.

coin - “litecoin.json” -> “bitcoinrand.json”.

address - “n4jSe18kZMCdGcZqaYprShXW6EH1wivUK1” -> Enter the receiving address from the RPC command “getaccountaddress ""”.

rewardRecipients - Remove all recipients.

minimumPayment - “70” -> “0.01”.

"paymentProcessing" -> port - “19332” -> “19451”.

"paymentProcessing" -> user - “testuser” -> “rpc_bitcoinrand”.

"paymentProcessing" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

"daemons" -> port - “19332” -> “19451”.

"daemons" -> user - “testuser” -> “rpc_bitcoinrand”.

"daemons" -> password - “testpass” -> “dR2oBQ3K1zYMZQtJFZeAerhWxaJ5Lqeq9J2”.

p2p - “true” -> “false”.

Save the file with the keyboard shortcut ctrl + x.

Type the following command to open a screen session:

screen

Type the following commands to start your mining pool:

cd $HOME/nomp
sudo node init.js

Press the keyboard shortcut ctrl + a + d to disconnect from your screen session.


Instructions to mine with your mining pool.

Open your wallet.

Go to Help -> Debug Window.
Click on the tab Console.
This is the console where you execute RPC commands.

Type the following command, to create a legacy receiving address for your miner:

getnewaddress "" "legacy"

Example output.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop

Click here to download cpuminer and extract the zip file.

Open "Run" with the keyboard shortcut winkey + r.

Enter the following text behind "Open": notepad

Press on the button "OK".

Modify the following values in the following text.

minerd -a sha256d -o stratum+tcp://203.0.113.53:3008 -u 4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop -p anything

203.0.113.53 - “203.0.113.53” -> IPv4 address of your VPS.

4UyrFQrAoNQMEMqNhZTareRmjeU68bLiop - “TP56yqPtRTkse49B96sHzo2B6v48MV24vP” -> Receiving address from the RPC command “getnewaddress” for your miner.

Paste the modified text into notepad.

Click on the menu item "File" -> "Save As...".

The Open dialog box will appear, click on "Save as type" and select the option "All Files (*.*)".

Enter the following text behind "File name": mine.bat

Click on the menu bar, open the directory where you extracted pooler-cpuminer-2.5.0-win64.zip and press on the enter key. enter

Press on the button "Save".

Execute mine.bat to mine with your mining pool.
