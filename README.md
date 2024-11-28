

![image](https://github.com/user-attachments/assets/c912adba-b9b2-431f-acd5-99f6f6ce6753)

 * [Topluluk kanalımız](https://t.me/corenodechat)<br>
 * [Topluluk Twitter](https://twitter.com/corenodeHQ)<br>
 * [Discord](https://discord.com/invite/0glabs)<br>

### Gereklilikler ve ayarlar
```
sudo apt update && sudo apt upgrade
```
```
sudo ufw allow 8002/tcp
sudo ufw allow 8003/tcp
```

### Dosyaları çekelim...
```
cd $HOME
sudo mkdir -p $HOME/opt/dcdn
```
NOT: alttaki varyasyonlara mailde alınan download yazan linkler eklenecek ilki pipe toll diğeri dcdnd mailde linke sağ tıkla bağlantıyo kopyala de kopyalar. TIRNAK necmi kalıyor arasına yazıyoruz.
### Değişkenleri atayalım
```
PIPE="LİNK-BURYA-YAZ"
DCDND="LİNK-BURYA-YAZ"
```
```
sudo wget -O $HOME/opt/dcdn/pipe-tool "$PIPE"
sudo wget -O $HOME/opt/dcdn/dcdnd "$DCDND"
```
### Yetkilendirme
```
sudo chmod +x $HOME/opt/dcdn/pipe-tool
sudo chmod +x $HOME/opt/dcdn/dcdnd
```
### Kısayol
```
sudo ln -s $HOME/opt/dcdn/pipe-tool /usr/local/bin/pipe-tool -f
sudo ln -s $HOME/opt/dcdn/dcdnd /usr/local/bin/dcdnd -f
```
### Servis
```
sudo tee /etc/systemd/system/dcdnd.service > /dev/null << EOF
[Unit]
Description=DCDN Node Service
After=network.target
Wants=network-online.target

[Service]
ExecStart=$(which dcdnd) \
                --grpc-server-url=0.0.0.0:8002 \
                --http-server-url=0.0.0.0:8003 \
                --node-registry-url="https://rpc.pipedev.network" \
                --cache-max-capacity-mb=1024 \
                --credentials-dir=/root/.permissionless \
                --allow-origin=*

Restart=always
RestartSec=5

LimitNOFILE=65536
LimitNPROC=4096

StandardOutput=journal
StandardError=journal
SyslogIdentifier=dcdn-node

WorkingDirectory=$HOME/opt/dcdn

[Install]
WantedBy=multi-user.target
EOF
```
### Login olma ve Token register
```
pipe-tool login --node-registry-url="https://rpc.pipedev.network"
```
NOT: karekod çıkıcak yukarsında adres ve bir kod siteye git kodu gir mail bağla tamamlanınca sunucuda tamamlanıcak.
```
pipe-tool generate-registration-token --node-registry-url="https://rpc.pipedev.network"
```
### servis başlat
```
sudo systemctl daemon-reload
sudo systemctl enable dcdnd
sudo systemctl restart dcdnd
```
### Loglar
NOT: bişi göstermıyor loglar hata olursa atuyor sanırım.
```
sudo journalctl -f -u dcdnd.service
```
## Cüzdan oluşturma
```
pipe-tool generate-wallet --node-registry-url="https://rpc.pipedev.network" --key-path=$HOME/.permissionless/key.json
```
### Cüzdan link
```
pipe-tool link-wallet --node-registry-url="https://rpc.pipedev.network" --key-path=$HOME/.permissionless/key.json
```
### Node listeleme
NOT: Aktive göruyorsak tamamız.
```
pipe-tool list-nodes --node-registry-url="https://rpc.pipedev.network"
```
### Yedeklemek için
```
pipe-tool show-private-key
```
```
pipe-tool show-public-key
```








