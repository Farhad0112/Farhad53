
for github


------------------------------------------
sudo apt update && sudo apt upgrade -y
---------------------------------------------
sudo apt install curl tar wget clang pkg-config libssl-dev jq build-essential bsdmainutils git make ncdu -y
-------------------------------------------------
cd $HOME
ver="1.18.3"
wget "https://golang.org/dl/go$ver.linux-amd64.tar.gz"
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf "go$ver.linux-amd64.tar.gz"
rm "go$ver.linux-amd64.tar.gz"
echo "export PATH=$PATH:/usr/local/go/bin:$HOME/go/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile

---------------------------------------------------
go version

--------------------------------------
cd $HOME
rm -rf celestia-app
git clone https://github.com/celestiaorg/celestia-app.git
cd celestia-app
git checkout v0.6.0
make install

-------------------------------------------------
celestia-appd version

-------------------------------------------
git clone https://github.com/celestiaorg/networks

-------------------------------
cd $HOME
rm -rf networks
git clone https://github.com/celestiaorg/networks.git

--------------------------
CELESTIA_NODENAME="MY_NODE" 
CELESTIA_WALLET="MY_WALLET"
CELESTIA_CHAIN="mamaki"

اطلاعات  داخل "" رو پاک کنید(فقط خط اول و دوم) و اطلاعات خودتون رو قرار بدید
به عنوان مثال :

CELESTIA_NODENAME="amir" 
CELESTIA_WALLET="amir8372"
CELESTIA_CHAIN="mamaki"

----------------------------------------------------------
echo 'export CELESTIA_CHAIN='$CELESTIA_CHAIN >> $HOME/.bash_profile
echo 'export CELESTIA_NODENAME='${CELESTIA_NODENAME} >> $HOME/.bash_profile
echo 'export CELESTIA_WALLET='${CELESTIA_WALLET} >> $HOME/.bash_profile
source $HOME/.bash_profile

------------------------------------------------
celestia-appd init $CELESTIA_NODENAME --chain-id $CELESTIA_CHAIN

-------------------------------------
cp $HOME/networks/mamaki/genesis.json $HOME/.celestia-app/config/

---------------------------------------
sed -i 's/mode = \"full\"/mode = \"validator\"/g' $HOME/.celestia-app/config/config.toml

---------------------------------------------
BOOTSTRAP_PEERS=$(curl -sL https://raw.githubusercontent.com/celestiaorg/networks/master/mamaki/bootstrap-peers.txt | tr -d '\n')

--------------------------------------------------
echo $BOOTSTRAP_PEERS

------------------------------------------------
sed -i.bak -e "s/^bootstrap-peers *=.*/bootstrap-peers = \"$BOOTSTRAP_PEERS\"/" $HOME/.celestia-app/config/config.toml

-----------------------------------------------------------
sed -i 's/timeout-commit = ".*/timeout-commit = "25s"/g' $HOME/.celestia-app/config/config.toml
sed -i 's/peer-gossip-sleep-duration *=.*/peer-gossip-sleep-duration = "2ms"/g' $HOME/.celestia-app/config/config.toml

-----------------------------------------------------------------
max_num_inbound_peers=40 
max_num_outbound_peers=10 
max_connections=50

----------------------------------------------------------------
sed -i -e "s/^use-legacy *=.*/use-legacy = false/;\
s/^max-num-inbound-peers *=.*/max-num-inbound-peers = $max_num_inbound_peers/;\
s/^max-num-outbound-peers *=.*/max-num-outbound-peers = $max_num_outbound_peers/;\
s/^max-connections *=.*/max-connections = $max_connections/" $HOME/.celestia-app/config/config.toml

------------------------------------------------
pruning_keep_recent="100" 
pruning_interval="10"

------------------------------------------------------

sed -i -e "s/^pruning *=.*/pruning = \"custom\"/;\
s/^pruning-keep-recent *=.*/pruning-keep-recent = \"$pruning_keep_recent\"/;\
s/^pruning-interval *=.*/pruning-interval = \"$pruning_interval\"/" $HOME/.celestia-app/config/app.toml

--------------------------------------------

sed -i 's/snapshot-interval *=.*/snapshot-interval = 0/' $HOME/.celestia-app/config/app.toml

---------------------------------------------------
celestia-appd tendermint unsafe-reset-all --home $HOME/.celestia-app

---------------------------------------------------

celestia-appd config chain-id $CELESTIA_CHAIN
celestia-appd config keyring-backend test

-------------------------------------------------------
tee $HOME/celestia-appd.service > /dev/null <<EOF
[Unit]
  Description=celestia-appd Cosmos daemon
  After=network-online.target
[Service]
  User=$USER
  ExecStart=$(which celestia-appd) start
  Restart=on-failure
  RestartSec=3
  LimitNOFILE=65535
[Install]
  WantedBy=multi-user.target
EOF

---------------------------------------------------
sudo mv $HOME/celestia-appd.service /etc/systemd/system/

----------------------------------------------------
sudo systemctl enable celestia-appd
sudo systemctl daemon-reload
sudo systemctl restart celestia-appd && journalctl -u celestia-appd -f -o cat

------------------------------------------------------------
Press CTRL+C to stop the logs

-------------------------------------------------------------

sudo systemctl stop celestia-appd

--------------------------------------------------------------

cd $HOME
rm -rf ~/.celestia-app/data
mkdir -p ~/.celestia-app/data
SNAP_NAME=$(curl -s https://snaps.qubelabs.io/celestia/ | \
    egrep -o ">mamaki.*tar" | tr -d ">")
wget -O - https://snaps.qubelabs.io/celestia/${SNAP_NAME} | tar xf - \
    -C ~/.celestia-app/data/

------------------------------------------------------------------
منتظر بمانید تا گره شما اکثر بلوک ها را از زنجیره بلوکی Celestia دانلود کند

--------------------------------------------------
sudo systemctl restart celestia-appd && journalctl -u celestia-appd -f -o cat

-------------------------------------------------
CTRL+C را فشار دهید تا لاگ ها متوقف شوند

---------------------------------
قبل از ادامه، با کد زیر مطمئن شوید که به طور کامل با شبکه همگام شده اید 

curl -s localhost:26657/status | grep block_height
------------------------------------------------
باید منتظر بمانید تا گره Validator شما همه بلوک‌ها را از زنجیره بلوکی سلستیا دانلود کند

---------------------------------------------------

celestia-appd keys add $CELESTIA_WALLET

اطلاعات دریافتی این بخش رو توی نوت پد برای خودتون ذخیره کنید

-----------------------------------
CELESTIA_ADDR=$(celestia-appd keys show $CELESTIA_WALLET -a) 
echo $CELESTIA_ADDR 
echo 'export CELESTIA_ADDR='${CELESTIA_ADDR} >> $HOME/.bash_prof

------------------------------------------------
CELESTIA_VALOPER=$(celestia-appd keys show $CELESTIA_WALLET --bech val -a) 
echo $CELESTIA_VALOPER 
echo 'export CELESTIA_VALOPER='${CELESTIA_VALOPER} >> $HOME/.bash_profile 
source $HOME/.bash_profile

--------------------------------------------
با لینک https://discord.gg/celestiacommunity وارد دیسکورد پروژه بشید و به بخش mamaki faucet  برید و به شکل زیر ادرس خودتون رو وارد کنید و درخواست فاست شبکه رو بدید
$request celestia1a35035fu83jkeeqz00d3jmt3k5wu5x3lyvn6qp

------------------------------------- 
با کد زیر هم میتونید موجودی ولت خودتون رو ببنید  که به جای ایکس ها ادرس خودتون رو وارد کنید 

celestia-appd q bank balances xxxxxxxxxxxxx
