# How to update CocoaPods

Execute the following on your terminal to get the latest stable version:

```bash sudo gem install cocoapods```

Add ```--pre``` to get the latest pre release:

```bash 
sudo gem install cocoapods --pre
```

### If there is any error happens

Try uninstall and install again:

```bash
sudo gem uninstall cocoapods
sudo gem install cocoapods
```

### Run after updating CocoaPods

```bash pod setup ```

If there is any error happens, try to remove cache and setup again

```bash
sudo rm -fr ~/Library/Caches/CocoaPods/
sudo rm -fr ~/.cocoapods/repos/master/
pod setup
```

### Remove all old versions of CocoaPods

```bash sudo gem clean cocoapods ```

### Update libraries (Pods) in your project

After updating CocoaPods, also need to update Podfile.lock file in your project.

- Go to your project directory
- Run

```pod install```

If there is any error happens, remove all old Pods and re-install.

```bash
sudo rm -fr Pods/
pod install
```
