<h1 align="center">BEVM Node Kurulumu</h1>

Donanım
2 CPU 2 RAM

Yeni bir sunucu almaya gerek yok.

<h1 align="center">Kurulum</h1>


Sunucu güncelleme

    ```
    sudo apt update
    sudo apt upgrade
    sudo apt install --assume-yes git clang curl libssl-dev llvm libudev-dev make protobuf-compiler
    sudo ufw allow ssh; sudo ufw allow 30333; sudo ufw allow 20222; sudo ufw allow 30334
    ```

# Binary çekme
mkdir -p $HOME/.bevm
wget -O bevm https://github.com/btclayer2/BEVM/releases/download/testnet-v0.1.1/bevm-v0.1.1-ubuntu20.04 && chmod +x bevm
sudo cp bevm /usr/bin/

# Servis oluşturma, mmadresinizi kendinize göre düzenleyin.
sudo tee /etc/systemd/system/bevm.service > /dev/null << EOF
[Unit]
Description=BEVM
After=network-online.target
StartLimitIntervalSec=0
[Service]
User=root
Restart=always
RestartSec=3
ExecStart=/usr/bin/bevm --chain=testnet --name="mmadresiniz" --pruning=archive --telemetry-url "wss://telemetry.bevm.io/submit 0"
[Install]
WantedBy=multi-user.target
EOF

# Servis başlatma
sudo systemctl daemon-reload
sudo systemctl enable bevm
sudo systemctl start bevm

# Çıktı kontrol, exit-code vermesin dikkat.
sudo journalctl -u bevm -f --no-hostname -o cat
```

> Telemetry kontrol
https://telemetry.bevm.io/#/0x41cfeafc7177775a0e838b3725a0178b89ebf5dde1b5f766becbf975a24e297b
