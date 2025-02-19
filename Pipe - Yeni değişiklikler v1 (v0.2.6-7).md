## Yeni değişiklikler v1
### Servisi güncelleyeceğiz genel bi düzenlemeleri yapcaz. İsteğe bağlı `--enable-80-443` flagı gelmiş. İsterseniz servise ekleyin. 80 ve 443 portlarınız doluysa mesela nginx varsa bu flagları kullanmayın. Kullanmayınca çalışıyor en azından. İlk önce servisi güncelleyip sonra versiyon güncelleyeceğiz.
Hatırlatma: Taşıma muhabbetini netleştirelim. `node_info.json` ile taşıyamıyorsunuz. Dökümanlarında ip'ye kayıt olduğunu ve yeni ipde çalışmayacağını söylemişler. Yedeğinizi aynı ipde kullanabiliyorsunuz.
#### Durdurup servisi güncelleyelim. Serviste düzenlenecek yerleri düzenlemeyi unutmayın.
```bash
sudo systemctl stop popd
```
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
```bash
sudo systemctl daemon-reload
```
#### Sonra kuruluma geçelim (Şuan v0.2.7 var)
```bash
cd $HOME
wget https://dl.pipecdn.app/v0.2.7/pop
chmod +x ./pop
mv $HOME/pop $HOME/pipe/pop
cd $HOME/pipe && ./pop --refresh
```
#### En son kontrol edelim
```bash
./pop --status
```
#### Bi de restart atalım
```bash
sudo systemctl restart popd
```
#### Logu kontrol edelim
```bash
sudo journalctl -u popd -fo cat --since "1 hour ago"
```
Tamamlandı!
