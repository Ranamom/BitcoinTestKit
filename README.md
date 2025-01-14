# Bitcoin Test Kit

This is a bitcoin regtest environment tuned for security and privacy tests. Original version by Aglietti and Barnini as precisely mentioned below. I had the honour of forking their original project in order to create a new version which is better tuned on my privacy&security analisys and didactics.

## Prerequisites install

This install has been tested on Ubuntu 20.04 running on a standard VPS. The distro needs docker fully running. This is the link for [ubuntu](https://docs.docker.com/engine/install/ubuntu/). A small recap of installing docker is here below:

```
sudo apt-get update
sudo apt-get install git curl wget
sudo apt-get install apt-transport-https ca-certificates curl gnupg lsb-release
```

Download the GPG key from the APT docker repo

```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg
```

Add APT repository that matches your CPU architecture

```
echo \
  "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

Install docker & docker-compose

```
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
sudo apt-get install docker-compose
```

## Kit install

First of all clone the repository

```
git clone https://github.com/massmux/BitcoinTestKit.git
```

Now ready for running containers:

if not already, put your user in docker group

```
adduser YOURUSER docker
```
Then you are ready to run containers

```
cd ~/BitcoinTestKit
docker-compose up
```

## Log into the "hansel" node

To log into the bash of the hansel node, do as follows


```
:~$ docker exec -ti hansel bash
:/opt/nodeworkdir$
```

## Mine the first block

The whole regtest is empty, there are no blocks. So the first step is create a new block by mining it. 

if no wallet is defined, create one:
```
bitcoin-cli createwallet "hansel"
```

then mine to any wallet's address

```
bitcoin-cli generatetoaddress 1 $(bitcoin-cli getnewaddress)
```

## Ports and services

- blockchain explorer interface is at port 8094
- spark wallet is at port 9737
- lightningd is available for channels opening at port 9735
- LND is available for channels opening at port 19735
- electrs is reachable on port 50001 for electrum connection

## What you can immediately do

- mine blocks
- connect a bitcoin-qt wallet
- connect electrum on port 50001
- open a channel to lightningd and to LND from electrum
- open a channel from LND to lightningd
- move funds onchain and LN
- query the blockchain through explorer and by using the bitcoin-cli


## Run on Google Cloud Shell

[![Open this project in Cloud
Shell](http://gstatic.com/cloudssh/images/open-btn.png)](https://console.cloud.google.com/cloudshell/open?git_repo=https://github.com/massmux/BitcoinTestKit.git&tutorial=googleshell.md&shellonly=true)

### How to connect to google cloud using ngrok

For connecting to google running the kit, from outside, we need to forward ports. In order to do that, we are using ngrok.

Install ngrok

```
wget https://bin.equinox.io/c/4VmDzA7iaHb/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
chmod +x ngrok
```

Connect you authtoken

```
./ngrok authtoken <yourtoken>
```

Now copy the content of ngrok.yml that you find in the repository to $HOME/.ngrok2/ngrok.yml paying attention not to overwrite the first line containing your authtoken.

Now run in this way

```
ngrok start electrs-50001 explorer-8094 lightningd-9735 lnd-19735
```
all tunnels defined are available in the ngrok.yml file

## Original version

This playground has been created originally by authors of books (in italian) "[Bitcoin dalla teoria alla pratica](https://www.amazon.com/Bitcoin-Dalla-teoria-pratica-Italian/dp/B07SNNNL2P)" / "[Bitcoin in Action](https://www.amazon.com/gp/product/B08NL5ZV6X)" and channel [Bitcoin in Action](https://www.youtube.com/BitcoinInAction). Therefore many many thanks to Aglietti & Barnini



