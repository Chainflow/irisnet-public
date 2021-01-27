

## Iris Alerting Bot

Today [Chainflow](https://chainflow.io/staking) and [Vitwit](https://vitwit.com) are releasing an Iris Network alerting bot. It's a simple, yet effective, open source bot that sends Telegram or email alerts based on key Iris validator performance metrics.

It also sends sanity checks twice per day. The sanity checks confirm the validator is voting as expected (if it is!).

The bot is an offshoot of the [Cosmos Validator Mission Control monitoring and alerting tool](https://chainflow.io/cosmos-validator-mission-control/) we built. You can find [a similar bot for Cosmos here](https://github.com/Chainflow/cosmos-validator-mission-control/tree/master/alert-bot).

We hope the Iris Network community finds this tool beneficial. It's part of our commitment to supporting the networks we believe in, within the resource constraints of a smaller, independent validator operator.

You can find the bot here. Please leave any feedback in the same repo.

P.S. - You can support this effort by [delegating to the Chainflow Iris Validator](https://iris.bigdipper.live/validator/iva1tsjrct9p7z2znsu4ehs69w5ydu5d5mu4sxst73) 🙏

*(Originally published [here](https://chainflow.io/iris-network-alerting-bot/).)*

 -   **Iris alerting bot** will send alerts to your telegram account about your **validator status**(jailed or voting) and about **missed blocks** (based on the configured thershold).

## Install Prerequisites
- **Go 14.x+**
- **InfluxDB**

### To install go if it's not installed in your system follow below steps

```sh
$ sudo apt update
$ sudo apt install build-essential jq -y

$ wget https://dl.google.com/go/go1.15.5.linux-amd64.tar.gz
$ tar -xvf go1.15.5.linux-amd64.tar.gz
$ sudo mv go /usr/local

Update bashrc

$ export GOPATH=$HOME/go
$ export GOROOT=/usr/local/go
$ export GOBIN=$GOPATH/bin
$ export PATH=$PATH:/usr/local/go/bin:$GOBIN
$ echo "" >> ~/.bashrc
$ echo 'export GOPATH=$HOME/go' >> ~/.bashrc
$ echo 'export GOROOT=/usr/local/go' >> ~/.bashrc
$ echo 'export GOBIN=$GOPATH/bin' >> ~/.bashrc
$ echo 'export PATH=$PATH:/usr/local/go/bin:$GOBIN' >> ~/.bashrc

$ source ~/.bashrc

```

### Install InfluxDB

```sh
$ wget https://dl.influxdata.com/influxdb/releases/influxdb_1.8.3_amd64.deb
$ sudo dpkg -i influxdb_1.8.3_amd64.deb
```

Start influxDB

```sh
$ sudo systemctl start influxdb 

The default port that runs the InfluxDB HTTP service is 8086
```

Create an influxDB database:

```sh
$   cd $HOME
$   influx
>   CREATE DATABASE iris_bot  
$   exit
```
### Getting Started

```bash
git clone https://github.com/Chainflow/irisnet-public.git
cd cirisnet-public
cp example.config.toml config.toml
```
### Configure the following variables in `config.toml`

- *tg_chat_id*

    Telegram chat ID to receive Telegram alerts, required for Telegram alerting.
    
- *tg_bot_token*

    Telegram bot token, required for Telegram alerting. The bot should be added to the chat and should have send message permission.

- *alert_time1* and *alert_time2*

    These are for regular status updates. To receive **validator status** daily (twice), configure these parameters in the form of "02:25PM". The time here refers to UTC time.

- *val_operator_addr*

    Operator address of your validator which will be used to get staking, delegation and distribution rewards.

- *validator_hex_addr*

    Validator hex address useful to know about last proposed block, missed blocks and voting power.

- *lcd_endpoint*

    Address of your lcd client (ex: http://localhost:1317).

- *external_rpc*

    External open RPC endpoint(secondary RPC other than your own validator). Useful to gather information like validator caught up, syncing and missed blocks etc.

- *missed_blocks_threshold*

    Configure the threshold to receive  **Missed Block Alerting**, e.g. a value of 10 would alert you every time you've missed 10 consecutive blocks.

After populating config.toml 

- Build and run the alerting bot using binary

```bash
$ go build -o iris-bot && ./iris-bot
```
