
''panic: UPGRADE "drs-4" NEEDED at height: 100: {}''  ROLLAPPINDA BU HATAYI ALMADAN UPDATE EDENLER
```
cd
mv rollapp-evm rollapp-evmydk1
git clone -b v3.0.0-rc01-drs-1 https://github.com/dymensionxyz/rollapp-evm.git
cd rollapp-evm
export BECH32_PREFIX=token-kısaltması && make build BECH32_PREFIX=$BECH32_PREFIX
sudo cp ./build/rollapp-evm $(which rollappd)
```
```
roller rollapp migrate
roller rollapp services start
```
