
''panic: UPGRADE "drs-4" NEEDED at height: 100: {}''  ROLLAPPINDA BU HATAYI ALMADAN UPDATE EDENLER

        rm -rf rollapp-evm
        rm -rf .rollapp_evm
        
        git clone -b v3.0.0-rc01-drs-1 https://github.com/dymensionxyz/rollapp-evm.git
        cd rollapp-evm
        export BECH32_PREFIX=token-kısaltması && make build BECH32_PREFIX=$BECH32_PREFIX
        sudo cp ./build/rollapp-evm $(which rollappd)

        roller rollapp migrate
        roller rollapp services start
