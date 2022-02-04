## Launch Your App on Devices Using Free Provisioning (iOS, watchOS)

1. If you don’t join the Apple Developer Program, you can still build and run your app on your devices using free provisioning. However, the capabilities available to your app, described in Adding Capabilities, are restricted when you don’t belong to the Apple Developer Program.
The precise steps to getting your app onto your iOS device or Apple Watch follow immediately thus (screenshots omitted for ease of skimming):

2. In Xcode, add your Apple ID to Accounts preferences, described in Adding Your Apple ID Account in Xcode.
3. In the project navigator, select the project and your target to display the project editor.
4. Click General and choose your name from the Team pop-up menu.
5. Connect the device to your Mac and choose your device from the Scheme toolbar menu.
6. Below the Team pop-up menu, click Fix Issue.

Xcode creates a free provisioning profile for you and the warning text under the Team pop-up menu disappears.
Click the Run button.

7.Xcode installs the app on the device before launching the app.

## Another option
> Navigate to /Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS4.2.sdk and open the file SDKSettings.plist.

In that file, expand DefaultProperties and change **CODE_SIGNING_REQUIRED** to **NO**, while you are there, 
you can also change **ENTITLEMENTS_REQUIRED** to NO also.
You will have to restart Xcode for the changes to take effect. Also, you must do this for every .sdk 
you want to be able to run on device.

Now, in your project settings, you can change **Code Signing Identity** to **Don't Code Sign**.

Your app should now build and install on your device successfully.

