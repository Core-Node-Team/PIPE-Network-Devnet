# PIPE DEVNET2

[Official Guide](https://docs.pipe.network/devnet-2)

# Dashboard

https://dashboard.pipenetwork.com/node-lookup

## Sistem güncellemesi

```bash
sudo apt update && sudo apt upgrade -y
```

## Gerekli port açılışı

```bash
sudo ufw allow 8003/tcp
```

## Dizinleri oluşturma

```bash
mkdir -p $HOME/download_cache
sudo mkdir -p $HOME/pipe
```

## POP binary indirme ve yetkilendirme (kurduğun zamanki son versiyona göre linki değiştirebilirsin.)

```bash
POP=https://dl.pipecdn.app/v0.2.7/pop
cd $HOME/pipe
wget -O pop "$POP"
chmod +x pop
```

## Servis dosyası oluşturma

- Burada servis içine ram, max kullanmasını istediğiniz disk ve cüzdan adresinizi yazacaksınız.pubkey yazın pubkey yane cüzdan adresidir bilocan

```bash
sudo tee /etc/systemd/system/popd.service > /dev/null << EOF
[Unit]
Description=Pipe POP Node Service
After=network.target
Wants=network-online.target

[Service]
User=$USER
ExecStart=$(echo $HOME)/pipe/pop --ram=12 --pubKey Solana_Adresi_Yaz --max-disk 175 --cache-dir $(echo $HOME)/download_cache --no-prompt
Restart=always
RestartSec=5
LimitNOFILE=65536
LimitNPROC=4096
StandardOutput=journal
StandardError=journal
SyslogIdentifier=pop-node
WorkingDirectory=$HOME/pipe
AmbientCapabilities=CAP_NET_BIND_SERVICE
CapabilityBoundingSet=CAP_NET_BIND_SERVICE

[Install]
WantedBy=multi-user.target
EOF
```

## İzinleri ayarlama

```bash
sudo chown -R $USER:$USER $HOME/pipe
sudo chown -R $USER:$USER $HOME/download_cache
```

## Servisi başlatma

```bash
sudo systemctl daemon-reload
sudo systemctl enable popd
sudo systemctl start popd
```

## Servis durumunu kontrol etme

```bash
sudo systemctl status popd
```

## Log takibi için

```bash
sudo journalctl -u popd -fo cat
```

## Diğer Komutlar
ilk önce `cd $HOME/pipe` sonra komutlar. Komutları kullanırken node_info.json'dan bilgi çekiyor. Bu yüzden ana dizinde direkt `pop` yazarak kullanamıyoruz.

- `./pop --status` metrik için

- `./pop --points-route` puan kontrolü için

- `./pop --gen-referral-route` referans kodu üretme

- `./pop --signup-by-referral-route KODUYAZ` ile de aldığınız referansla kaydolabilirsiniz. `888085259b07a50f` kullanmak isterseniz benim ürettiğim budur. Bunu kullanabilmeniz için muhtemelen `sudo systemctl start popd` ile başlatmadan önce dizinde kodu gimeniz gerekebilir.

Not: 2.50'de mail geldi. Sabaha doğru 4.50 gibi bitti. Uykusuzlukla hata yaptıysam affola.
