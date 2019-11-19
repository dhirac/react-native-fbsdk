# react-native-fbsdk
Install React Native FBSDK in IOS with CocoaPods


For those who are facing with the problem to install FBSDK in their react native projects, this is the working solutions.

# Major Error
null is not an object (evaluating 'loginmanager.login with permissions')

In the latest react native version >= 0.60.0 when you install FBSDK you will probably get this error and having hard time figuring out how to solve this issues.

# Working Solution.

1) creaete new react native project or existing.
# react-native init AwesomeProject 

2) We are using CocoaPods to install FBSDK, so if you have already installed then its fine if not lets install it.

# brew install cocoapods

3) Open the ios project

# open ios/AwesomeProject.xcworkspace

4) Add this line in your PodFile
# pod 'react-native-fbsdk', :path => '../node_modules/react-native-fbsdk'

5) Run this going to your ios project
# cd /ios
# pod install

 So if you get any error reagarding react-native-fbsdk then install and no need to link it will link automatically.
 
 # npm install --save react-native-fbsdk
 
 then again run pod install it will run the pod and install the FBSDK core libray.
 
 6) open AppDelegate.m file in your ios project

# import <FBSDKCoreKit/FBSDKCoreKit.h>
# import <FBSDKLoginKit/FBSDKLoginKit.h>
# import <FBSDKShareKit/FBSDKShareKit.h>


Then add this line to the beginning of the didFinishLaunchingWithOptions function:

[[FBSDKApplicationDelegate sharedInstance] application:application
 didFinishLaunchingWithOptions:launchOptions];
 
 And add this function before end
 
 - (BOOL)application:(UIApplication *)application
 openURL:(NSURL *)url
 options:(NSDictionary<UIApplicationOpenURLOptionsKey,id> *)options {
BOOL handled = [[FBSDKApplicationDelegate sharedInstance] application:application
 openURL:url
 sourceApplication:options[UIApplicationOpenURLOptionsSourceApplicationKey]
 annotation:options[UIApplicationOpenURLOptionsAnnotationKey]
 ];
 // Add any custom logic here.
 return handled;
}


7)Go to the info.plist and add your facebook app keys, like this: (You have to login to facebook developer account and get it from there)

<key>CFBundleURLTypes</key>
<array>
 <dict>
 <key>CFBundleURLSchemes</key>
 <array>
 <string>fb{your-app-id}</string>
 </array>
 </dict>
</array>
<key>FacebookAppID</key>
<string>{your-app-id}</string>
<key>FacebookDisplayName</key>
<string>{your-app-name}</string>
<key>LSApplicationQueriesSchemes</key>
<array>
 <string>fbapi</string>
 <string>fb-messenger-api</string>
 <string>fbauth2</string>
 <string>fbshareextension</string>
</array>


Get your keys by accessing this link: https://developers.facebook.com/apps


And that’s it, you are DONE ! 🎉, now you can run your react native project, add button to  call  the function.

// ...

import { LoginManager } from "react-native-fbsdk";

// ...

_fbAuth(){
  
    // Attempt a login using the Facebook login dialog asking for default permissions.
LoginManager.logInWithPermissions(['public_profile','email']).then(
  function(result) {
    if (result.isCancelled) {
      console.log("Login cancelled");
    } else {
      console.log(
        "Login success with permissions: " +
          result.grantedPermissions.toString()
      );
    }
  },
  function(error) {
    console.log("Login fail with error: " + error);
  }
);


// in the view

        <TouchableOpacity style={styles.btnFacebook} onPress={this._fbAuth}>
             <Text>Log in with Facebook</Text>
        </TouchableOpacity>

 
 
 
