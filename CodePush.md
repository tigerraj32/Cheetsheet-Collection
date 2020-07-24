# Introduction

**CodePush** is an App Center Colud service provided by Microsoft that enables React Native / Cordova developers to deploy  mobile app updates directly to  their 
user's device without going throuch the google play store / Apple app store and review process. App Center provides other services as well such as 
- building
- testing, 
- distributing  and 
-  monitoring app in cloud.


# How CodePush works

Generally react native app in production consist of JSBundle file (i.e is the javascript files with images and other assets) which are bundle together by packager and distributed as part of a platform-specific binary (an .ipa or .apk file). When the app is released, updating either the JavaScript code (for example making bug fixes, adding new features) or image assets, requires you to recompile and redistribute the entire binary, which includes any review time associated with the stores to which you are publishing.

The CodePush plugin helps get product improvements in front of your end users instantly, by keeping your JavaScript and images synchronized with updates you release to the CodePush server. This way, your app gets the benefits of an offline mobile experience, as well as the "web-like" agility of side-loading updates as soon as they are available.

In order to ensure that your end users always have a functioning version of your app, the CodePush plugin maintains a copy of the previous update, so that in the event that you accidentally push an update that includes a crash, it can automatically roll back. This way, you can rest assured that your newfound release agility won't result in users becoming blocked before you have a chance to [roll back]() on the server.

# Get Started

## 1. Setup React Native Project

### 1.1. Create react native project

Open a terminal and execute following command to create react native project

        npx react-native init  CodePushDemo

### 1.2. Create git repository

- Go to https://github.com and create new repository with name **CodePushDemo**
- After that, go to root directory of just created CodePushDemo Pfoject and execute following command

        git init
        git add .
        git commit -m "first commit"
        git remote add origin https://github.com/{username}/CodePushDemo.git
        git push origin master

## 2. Setup App Center

### 2.1. Create account in App Center
- Create account or social login (github, facebook, google) in https://appcenter.ms/ 

### 2.2 Install the App Center CLI

- You manage most of CodePush's functionality using the App Center CLI. Open a terminal and execute the following command

        npm install -g appcenter-cli

### 2.3. Login to App Center

Before you can begin releasing app updates, you must sign-in with your existing CodePush account or create a new App Center account. You can do this by running the following command once you've installed the CLI:

        appcenter login

This command will launch a browser, asking you to authenticate with either your GitHub or Microsoft account. Once authenticated, it will create a CodePush account "linked" to your GitHub/MSA identity, and generate an access key you can copy/paste into the CLI in order to sign-in.

### 2.4. Access Tokens

To authenticate against the CodePush service without launching a browser and/or without needing to use your GitHub and/or Microsoft credentials (for example, in a CI environment), you can execute the following command to create an "access token" 

        appcenter tokens create -d "Azure DevOps Integration"

        appcenter login --token <accessToken>


**Additional commands**

        appcenter profile list  // View User profile

        appcenter logout        // logout from app center

        appcenter tokens list   //View running user session from all the machine

        appcenter tokens delete <tokenid>  // delete the user session

### 2.5. App Management 

**Create App** [doc](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/cli)

Before deploying updates, you must create an app with App Center for each platform (iOS and Android)

        appcenter apps create -d <appDisplayName> -o <operatingSystem>  -p <platform> 

        appcenter apps create -d AppUpdateDemoIOS  -o iOS  -p React-Native

        appcenter apps create -d AppUpdateDemoAndroid  -o Android  -p React-Native

If you decide that you don't like the name you gave to an app, you can rename it at any time using the following command:

        appcenter apps update -n <newName> -a <ownerName>/<appName>

If at some point you no longer need an app, you can remove it from the server using the following command:
        
        appcenter apps delete -a <ownerName>/<appName>

After you create the deployments, you can access the deployment keys for both deployments using

        appcenter apps list        

**Create deployment key** [docs](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/cli)

We can create deployment keys for production and staging from below command

        appcenter codepush deployment add -a <ownerName>/<appName> Staging
       
        appcenter codepush deployment add -a <ownerName>/<appName> Production

We can get the **<ownerName>/<appName>** pair by running following command

        appcenter apps list

        // returns rajan.twanabashu-zakipoint.com/AudoUpdatePush

To get the list of deployment keys 

        appcenter codepush deployment list  -a <ownerName>/<appName>  --displayKeys


or just this to view deployment metrics
        appcenter codepush deployment list  -a <ownerName>/<appName>

To rename or delete deployment keys

        appcenter codepush deployment remove -a <ownerName>/<appName> <deploymentName>
        appcenter codepush deployment rename -a <ownerName>/<appName> <deploymentName> <newDeploymentName>    


 **Dsipatch release via code push** 
 
 Once your app has been configured to query for updates against the App Center server, you can begin releasing updates to it.

        appcenter codepush release-react -a <ownerName>/<appName> -d <deploymentName>
        
        //e.g.
        appcenter codepush release-react -a rajan.twanabashu-zakipoint.com/AudoUpdatePush  -d Production
        
**Rollback release update**

A deployment's release history is immutable, so you cannot delete or remove an update once it has been released. However, if you release an update that is broken or contains unintended features, it is easy to roll it back using the rollback command:


        appcenter codepush rollback <ownerName>/<appName> <deploymentName>

        //e.g.
        appcenter codepush rollback  -a rajan.twanabashu-zakipoint.com/AudoUpdatePush  Production

 **Promote stagging update to production**       

        appcenter codepush promote -a <ownerName>/MyApp-iOS -s Staging -d Production -t "*"


 ## 3. Integrating Code Push : [docs](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/rn-get-started)

 Once we have configured the App Center, its time to integrate code-push into the react native application. Execute below command in terminal

        npm install --save react-native-code-push

### 3.1. iOS Setup               

Once you have the CodePush plugin, you must integrate it into the Xcode project of your React Native app and configure it correctly. Code push is not compactible with autolink in react native at this moment.  So execute following command to link 

    react-native link react-native-code-push


or update the Podfile to include 

    pod 'CodePush', :path => '../node_modules/react-native-code-push'

Finally from root directory install pod dependencues

    npx pod-install

    //or 
    cd ios
    pod install

**Integrate in Xcode project**

Open up the AppDelegate.m file, and add an import statement for the CodePush headers:

        #import <CodePush/CodePush.h>

Find the following line of code, which sets the source URL for bridge for production releases:        

        return [[NSBundle mainBundle] URLForResource:@"main" withExtension:@"jsbundle"];

and replace with

    return [CodePush bundleURL];

your code should now look like this 

```C
- (NSURL *)sourceURLForBridge:(RCTBridge *)bridge
{
  #if DEBUG
    return [[RCTBundleURLProvider sharedSettings] jsBundleURLForBundleRoot:@"index" fallbackResource:nil];
  #else
    return [CodePush bundleURL];
  #endif
}
```

**Using Deployment Kewy**

To let the CodePush runtime know which deployment it should query for updates against, open the project's **Info.plist** file and add a new entry named CodePushDeploymentKey, whose value is the key of the deployment you want to configure this app against

```plist
    <key>CodePushDeploymentKey</key>
	<string>R7Ms5WZiHbQ4fl4XB4_8HsAhPcm9ky53Ck1Am</string>
```

CodePush plugin makes HTTPS requests to the following domains:
        
        - codepush.appcenter.ms
        - codepush.blob.core.windows.net
        - codepushupdates.azureedge.net

If you want to change the default HTTP security configuration for any of these domains, you have to define the NSAppTransportSecurity (ATS) configuration inside the project's Info.plist file:

```plist
<plist version="1.0">
  <dict>
    <!-- ...other configs... -->

    <key>NSAppTransportSecurity</key>
    <dict>
      <key>NSExceptionDomains</key>
      <dict>
        <key>codepush.appcenter.ms</key>
        <dict><!-- read the ATS Apple Docs for available options --></dict>
      </dict>
    </dict>

    <!-- ...other configs... -->
  </dict>
</plist>
```

## 4. Installing the new update [docs](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/tutorials)

CodePush provides Cordova and React Native developers with multiple options to configure the end users update experience.
There are three potential "update modes" or deployment strategies for CodePush updates: Silent, Active, and Custom. 

### 4.1 Silence Mode

First of all in **App.js** we need to wrap out app inside CodePush higher order functions to configure the update check frequency.  

```js
const codePushOption = {
  checkFrequency: CodePush.CheckFrequency.ON_APP_RESUME,
};

............

export default CodePush(codePushOption)(App);

```

App will check for new update from appcenter server on every resume

To sync and install the available update we will need to add following code which will install the update after 10 seconds of app inactivity in background

```js
 CodePush.sync({ installMode: CodePush.InstallMode.ON_NEXT_RESUME, minimumBackgroundDuration: 10}, codepushSyncStatus, null); //10 sec
```

Here **CodePush.sync** accept following parameter
- installMode: apply update when the app is resume next time
- minimumBackgroundDuration : after this time of app in background new updates are applied
- codepushSyncStatus: call back function that will show the sync statys
- null : downloadProgress indicator. which we are not using at this moment. So ignore.


Full source code will look like this

```js
import CodePush from 'react-native-code-push'

const codePushOption = {
  checkFrequency: CodePush.CheckFrequency.ON_APP_RESUME,
};

const App: () => React$Node = () => {

  useEffect(() => {
    //app resumes after 10 minutes
    CodePush.sync({ installMode: CodePush.InstallMode.ON_NEXT_RESUME, minimumBackgroundDuration: 10}, codepushSyncStatus, null); //10 sec
    
  }, [])

  const codepushSyncStatus = (status) => {
    console.log("code push sync status", status)
  }
  
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <View style={{flex: 1,justifyContent: 'center',alignItems: 'center'}}>
        <Text> Rajan Twanabashu</Text>
      </View>
    </>
  );
};

export default CodePush(codePushOption)(App);

```


**Code cleanup** 

- create codepushwrapper.js

```js
import React,  {useEffect}from 'react';
import CodePush from 'react-native-code-push'

const codePushOption = {
  checkFrequency: CodePush.CheckFrequency.ON_APP_RESUME,
};


const CodePushWrapper = (WrappedComponent) => {
    
    const WrappedApp = ()=>{
        useEffect(() => {
            //app resumes after 10 minutes
            CodePush.sync({ installMode: CodePush.InstallMode.ON_NEXT_RESUME, minimumBackgroundDuration: 10}, codepushSyncStatus, null); //10 sec
            
          }, [])
        
          const codepushSyncStatus = (status) => {
            console.log("code push sync status", status)
          }
          return <WrappedComponent />
    }
    
    return CodePush(codePushOption)(WrappedApp);
}

export default CodePushWrapper
```

then we can cleanup **app.js** as following

```js
import CodePushWrapper from './codepush';
const App: () => React$Node = () => {
  
  return (
    <>
      <StatusBar barStyle="dark-content" />
      <View style={{flex: 1,justifyContent: 'center',alignItems: 'center'}}>
        <Text> Rajan Twanabashu</Text>
      </View>
    </>
  );
};

export default CodePushWrapper(App);
```


## 5. Conclusion

Now after all the above configuration we can dispatch the new release to appcenter via following command. Then depending on the codepush option your update will be silently installed at client end.

        appcenter codepush release-react -a <ownerName>/<appName> -d <deploymentName>



