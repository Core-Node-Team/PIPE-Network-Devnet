# PIPE GÜNCELLEME -1

## Dede ile ilk önce dashboard'dan güncelleme linkimizi alalım. Komutun çıktısındaki url'yi kopyalayın bi kenara.

```bash
cd $HOME/pipe && ./pop --status
```

## Sonra kuruluma geçelim

```bash
cd $HOME
wget dashborddaki-url
chmod +x ./pop
mv $HOME/pop $HOME/pipe/pop
cd $HOME/pipe && ./pop --refresh
```

## En son kontrol edelim

```bash
./pop --status
```

Tamamlandı!