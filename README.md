
![image](https://github.com/user-attachments/assets/bece2e89-bf0d-4b3f-876b-e4d46167abd3)


### Create Validator
👉❗NOT: cüzdan adi yaz
```
rollappd keys add cüzdan-adi-yaz --keyring-backend test
```
👉❗NOT: chain idnizi değiştirin
```
rollappd init molla202 --chain-id=gokhanmolla_899158-1
```
👉❗NOT: moniker-cüzdanadi-ve coin sembolü değiştiriniz
```
rollappd tx staking create-validator \
    --amount=1000000000000000000amll \
    --moniker=molla202 \
    --chain-id=gokhanmolla_899158-1 \
    --from=molla202 \
    --keyring-backend test \
    --commission-rate=0.05 \
    --commission-max-rate=0.20 \
    --commission-max-change-rate=0.01 \
    --min-self-delegation=1 \
    --pubkey=$(rollappd dymint show-sequencer) \
    --node=http://localhost:26657 \
    --fees 4000000000000amll --gas auto --gas-adjustment 1.3 -y
```
### DSR Kontrol

rollappd q rollappparams params

👉❗NOT: çıktı bele ise devam

![image](https://github.com/user-attachments/assets/2c9dfdd0-28f7-4110-b6bf-5ea97e618ddb)


### GOV
```
rollappd q auth module-account gov
```
👉❗NOT: çıkan adresi aşağıdaki `<authority-address>` adres kısmına yazıyoruz tarihi ayarlıyoruz(utc saat dilimine göre biz +3 öndeyiz) blok yuksekliği için(gerekli olmayabilir) `rollappd q block | jq -r '.block.header.height'` yazarak öğrenebilirsiniz. 10000000amll yazan yeri sizin coin sembolüne göre değiştiriceksiniz mesela abu ise aabu olacak gibi.
```
nano proposal.json
```
```
{
  "title": "Update rollapp to DRS-4",
  "description": "Upgrade Dymension rollapp to version DRS-4 scheduled upgrade time",
  "summary": "This proposal aims to upgrade the Dymension rollapp to version DRS-4, implementing new features and improvements, with a scheduled upgrade time.",
  "messages": [
    {
      "@type": "/rollapp.timeupgrade.types.MsgSoftwareUpgrade",
      "original_upgrade": {
        "authority": "<authority-address>",
        "plan": {
          "name": "drs-4",
          "time": "0001-01-01T00:00:00Z",
          "height": "1800",
          "info": "{}",
          "upgraded_client_state": null
        }
      },
      "upgrade_time": "2024-09-06T18:10:00Z"
    }
  ],
  "deposit": "10000000amll",
  "expedited": true
}
```


### GOV create

👉❗NOT: --from kısmındaki 🌟molla202 yerine olusturduğunuz cüzdan adını yazınız. aynı zamanda `coin sembolünüde` değiştiriniz. ❗rollapp-ağ-adı da yazınız
```
rollappd tx gov submit-proposal proposal.json --from molla202 --keyring-backend test --fees 4000000000000amll --gas auto --gas-adjustment 1.3 --chain-id rollapp-ağ-adı -b block
```
👉❗NOT: şimdi oluşturduğumuz oylamaya bakalım. id öğrenelim daha önce oluşturmadıysak 1 dir de yinede hata alırız bişi olur son proposal id öğrenmekte fayda var.
```
rollappd query gov proposals
```
#### GOV Vote
👉❗NOT: aynı zamanda sitedende göruncek bu oylama ordanda `deploy ettiğiniz cüzdanlada oy kullanırsınız` extradan.görunürse tabi.
```
rollappd tx gov vote 1 yes --from molla202 --keyring-backend test --fees 4000000000000amll --gas auto --gas-adjustment 1.3 --chain-id rollapp-ağ-adı -b block
```
👉❗NOT: aşağıdaki kodla tekrar bakalım oylama geçmişmi biraz zamn alır malum geçince bele göstericek
```
rollappd query gov proposals
```
![image](https://github.com/user-attachments/assets/d97678ac-bf4a-4653-bfcc-b24a37346496)

👉❗NOT: oylama oluşturuldu oylandı ve geçti dediğimiz saat ve blokda rollap loglarında bir uyarı mesajı göreceğiz panic: `UPGRADE "drs-4" NEEDED at height: 100: {}` rollapp durucak. güncellemeye geçiyoruz.
👉❗NOT: loglarımıza bakalım hatamız varmı `journalctl -fu rollapp`
👉❗NOT: uyarıda drs-4-1 yazdığına bakmayın ben kurcukladım oylama adı öle die öle oldu sizde drs-4 yazacak.

![image](https://github.com/user-attachments/assets/0fde53fd-afa3-4ecc-b5e0-01f94948ce9f)

```
roller rollapp services stop
```
👉❗NOT: buraya-yaz kısmını   coin sembolünüzse     değiştirin.   mesela abu sa abu yazın.
```
git clone -b v3.0.0-rc07-drs-4 https://github.com/dymensionxyz/rollapp-evm.git
cd rollapp-evm
export BECH32_PREFIX=buraya-yaz && make build BECH32_PREFIX=$BECH32_PREFIX
sudo cp ./build/rollapp-evm $(which rollappd)
```
```
roller rollapp migrate
```

![image](https://github.com/user-attachments/assets/f589502e-4255-43de-b194-738395f84cdc)



![image](https://github.com/user-attachments/assets/e80ba421-52dc-45d4-8848-42dfe89e9e4f)


```
roller rollapp services start
```

👉bakalım ne olmuş.
```
rollappd q rollappparams params
```







