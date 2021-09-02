# React Native Cheetsheet


## Installing React Native

### Install Homebrew

> /usr/bin/ruby -e “$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

### Install node.js

> brew install node

### Install watchman

> brew install watchman

### Install React Native CLI

> npm install -g react-native-cli




## Project Setup


### Create react native project

> npx react-native init {ProjectName}

## Run app in ios simulator

    cd {ProjectName}
    npx react-native run-ios

### To specify simulator device

    //Get List of simulator available 
    xcrun simctl list devices 

    //Then choose any one of them
    npx react-native run-ios --simulator "iPhone 11"

### To choose scheme / target

If you have multiple target for your project then

> npx react-native run-ios --scheme {Target Name}
