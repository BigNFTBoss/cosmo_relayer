# cosmo_relayer
BUILD FROM SOURCE
```
# install rust
curl https://sh.rustup.rs -sSf | sh
source "$HOME/.cargo/env"

git clone https://github.com/informalsystems/hermes.git
cd hermes
cargo build --release --bin hermes
# or build without telemetry
# cargo build --release --no-default-features --bin hermes

cd $HOME
mkdir -p $HOME/.hermes/bin

mv $HOME/hermes/target/release/hermes $HOME/.hermes/bin/
```
ADD TO PATH
```
echo "export PATH=$PATH:$HOME/.hermes/bin" >> $HOME/.bash_profile
source $HOME/.bash_profile

hermes version
```
CREATE UNIT
```
tee $HOME/hermesd.service > /dev/null <<EOF
[Unit]
Description=HERMES
After=network.target
[Service]
Type=simple
User=$USER
ExecStart=$(which hermes) start
Restart=on-failure
RestartSec=10
LimitNOFILE=65535
[Install]
WantedBy=multi-user.target
EOF

sudo mv $HOME/hermesd.service /etc/systemd/system/

# start service
sudo systemctl daemon-reload
sudo systemctl enable hermesd
```
