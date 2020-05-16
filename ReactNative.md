Before we start installing react native we need to setup 
- JDK
- Node
- Watchman
- Android SDK
- iOS SDK
- Cocoapod


Here we will go through the installation of its dependency.

## Installing Java / JDK

```bash
brew cask install java
```

### Verify java version

```bash
brew cask info java
```

### Setting JAVA_HOME

Normally **$PATH** path variable are can be placed in **~/.bash_profile or ~/.bashrc or ~/.zshrc** depending on the type of terminal tool used. So it better to put following command in all of terminal profile file. 

- .bash_profile :- bash_profile is executed for login shells
- .bashrc :- .bashrc is executed for interactive non-login shells.
- .zshrc:- is executed for zsh type of shell.

To find type of shell you are using 

> echo $SHELL 

```bash
export JAVA_HOME=/Library/Java/JavaVirtualMachines/adoptopenjdk-8.jdk/Contents/Home
export PATH=$PATH:$JAVA_HOME
```

Then make the change permanent
```bash
$ source ~/.bash_profile
```


## Install Android SDK

You can download Android Studio and then install Android SDK, Android NDK, gradle from there or we can use brew for this

```bash
brew install ant
brew install maven
brew install gradle
brew install android-sdk
brew install android-ndk
```

### Update enviromental variable

Open **~/.bash_profile or ~/.bashrc or ~/.zshrc**  and write following code

```bash
export ANT_HOME=/usr/local/opt/ant
export MAVEN_HOME=/usr/local/opt/maven
export GRADLE_HOME=/usr/local/opt/gradle
export ANDROID_HOME=/usr/local/opt/android-sdk
export ANDROID_NDK_HOME=/usr/local/opt/android-ndk
```


## Update Path

```bash

export PATH=$ANT_HOME/bin:$PATH
export PATH=$MAVEN_HOME/bin:$PATH
export PATH=$GRADLE_HOME/bin:$PATH
export PATH=$PATH:$ANDROID_HOME/emulator
export PATH=$PATH:$ANDROID_HOME/tools
export PATH=$PATH:$ANDROID_HOME/tools/bin
export PATH=$PATH:$ANDROID_HOME/platform-tools
```

Then make the change permanent
```bash
$ source ~/.bash_profile
```


## Install Cocoapod

```bash
$ sudo gem install cocoapods
```

## Install node, watchman
```bash
$ brew install node
$ brew install watchman
```

## Install react native

It is recommended to use yarn over npm by facebook.  We can install **react native cli** via

```bash
$ npm install -g yarn
$ yarn global add react-native-cli
```

Now to create react native project

```bash
react-native init ProjectName
```

or we can use npx directly to create react native project

```bash
$ npx react-native init AwesomeProject
```


## react-native doctor: React Native Diagnostic tool

**react-native doctor**, a new command to help you out with getting started, troubleshooting and automatically fixing errors with your development environment. 

```bash
$ npx @react-native-community/cli doctor

```

## Running react native project

```bash
$ cd ProjectName
$ npx react-native start
$ npx react-native run-ios
$ npx react-native run-android
```

## React Native Debugger

```bash
$ brew cask install react-native-debugger
```


=======
## Common Issue 

### Gradle

JDK 14 does not support gradle version less than 6.3. So it is required to use gradle version greater than 6.3. 
For this 
- ```cd android```
- In ```gradle-wrapper.properties``` please use grade version 6.3 or above.

```
distributionBase=GRADLE_USER_HOME
distributionPath=wrapper/dists
distributionUrl=https\://services.gradle.org/distributions/gradle-6.4.1-all.zip
zipStoreBase=GRADLE_USER_HOME
zipStorePath=wrapper/dists

```
### Error: spawn ./gradlew EACCES
 This require to change permission to **gradlew**
 
 - ```$ cd android ```
 - ```$ chmod +x gradlew```

