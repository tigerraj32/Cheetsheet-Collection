# View content of Keychain

We can view the content of View content of Keychain in iOS Simulator by following ways

 - `cd ~/Library/Developer/CoreSimulator/Devices/`
 - `xcrun simctl list | egrep '(Booted)'`.   Find the UDID of device that you are currently using
 - Open folder that matched UDID 
 - `cd data/Library/Keychains`
 - Here you will see `keychain-2-debug.db` this contains the keychain data.
    
